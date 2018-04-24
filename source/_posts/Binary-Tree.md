---
title: Binary Tree
date: 2016-12-19 12:39:03
tags: 
---
`19 Dec 2016`

# An implementation of Tree in C++
This is an simple implementation of Binary Tree template.

<!-- more -->
```c++
template <class T>
class BinaryTreeNode  
{
private:
    T  element;                                         //二叉树结点数据域
    BinaryTreeNode<T>*  left;                           //二叉树结点指向左子树的指针
    BinaryTreeNode<T>*  right;                          //二叉树结点指向左子树的指针
public:
    BinaryTreeNode();
    BinaryTreeNode(const T& ele);                       //给定数据的构造函数
    T  value() const;                                   //返回当前结点的数据
    void  setLeftchild(BinaryTreeNode<T>*);             //设置当前结点的左子树
    void  setRightchild(BinaryTreeNode<T>*);            //设置当前结点的右子树
    void  setValue(const T& val);                        //设置数据域
    BinaryTreeNode<T>*  leftchild() const;              //返回当前结点指向左子树的指针
    BinaryTreeNode<T>*  rightchild() const;             //返回当前结点指向右子树的指针
};

template <class T>
class BinaryTree  
{
private:
    BinaryTreeNode<T>*  root;                           //二叉树根结点指针
    vector<T> elements;  //用于保存遍历的元素
private:
    void Visit(T elem) {elements.push_back(elem);};     //遍历访问元素的值，返回数组线性表示遍历结果
    void PreOrder(BinaryTreeNode<T>* root);             //从root前序遍历二叉树或其子树(递归部分)
    void InOrder(BinaryTreeNode<T> *root);              //中序遍历
    void PostOrder(BinaryTreeNode<T> *root);            //后序遍历
    void LevelOrder(BinaryTreeNode<T> *root);           //序遍历
    void DeleteBinaryTree(BinaryTreeNode<T>* root);     //递归删除二叉树或其子树 
public:
    BinaryTree(){root=NULL;};                           //构造函数
    virtual ~BinaryTree(){DeleteBinaryTree(root);};                 //析构函数
    void Initialize(BinaryTreeNode<T>* pointer) {root=pointer;};    //构造树
    vector<T> traversePreOrder();  //从根节点前序遍历，以下依次对应
};

/*****************BinaryTreeNode类模板的实现************************/
template<class T>
BinaryTreeNode<T>::BinaryTreeNode()
{
    left=right=NULL;
}

template<class T>
BinaryTreeNode<T>::BinaryTreeNode(const T& ele) //给定数据的构造函数
{
    element=ele;
    left=right=NULL;
}

template<class T>
T  BinaryTreeNode<T>::value() const
{
    return element; 
}

template<class T>
BinaryTreeNode<T>*  BinaryTreeNode<T>::leftchild() const
{
    return left;                                //返回当前结点指向左子树的指针
}

template<class T>
BinaryTreeNode<T>*  BinaryTreeNode<T>::rightchild() const
{
    return right;                               //返回当前结点指向右子树的指针
}

template<class T>
void  BinaryTreeNode<T>::setValue(const T& val)     //赋值
{
    element = val;
}

template<class T>
void  BinaryTreeNode<T>::setLeftchild(BinaryTreeNode<T>* subroot)//设置当前结点的左子树
{
    left=subroot;
}

template<class T>
void  BinaryTreeNode<T>::setRightchild(BinaryTreeNode<T>* subroot)//设置当前结点的右子树
{
    right=subroot;
}

/*****************BinaryTree类模板的实现************************/

template<class T>
void BinaryTree<T>::DeleteBinaryTree(BinaryTreeNode<T>* root)       //递归删除二叉树或其子树
{
    if(root)
    {
        DeleteBinaryTree(root->leftchild());
        DeleteBinaryTree(root->rightchild());
        delete root;
    };
};


template<class T>
void BinaryTree<T>::PreOrder(BinaryTreeNode<T>* root)               //前序遍历二叉树或其子树
{
    if(root!=NULL)
    {
        Visit(root->value());   
        PreOrder(root->leftchild());            //访问左子树
        PreOrder(root->rightchild());           //访问右子树
    }
};

template<class T>
void BinaryTree<T>::InOrder(BinaryTreeNode<T>* root)               //前序遍历二叉树或其子树
{
    if(root!=NULL)
    {
        PreOrder(root->leftchild());            //访问左子树
        Visit(root->value());   
        PreOrder(root->rightchild());           //访问右子树
    }
};
template<class T>
void BinaryTree<T>::PostOrder(BinaryTreeNode<T>* root)               //前序遍历二叉树或其子树
{
    if(root!=NULL)
    {
        PreOrder(root->leftchild());            //访问左子树
        PreOrder(root->rightchild());           //访问右子树
        Visit(root->value());   
    }
};

template<class T>
vector<T> BinaryTree<T>::traversePreOrder() 
{
    elements.clear();
    PreOrder(root);
    return elements;
}
```

# rebuild binary tree

Given Inorder traverse array and PostOrder traverse array, rebuild the Binary Tree.

The idea is that:
    In order to build the Binary tree from pre/post Order and InOrder array,we can do the recursive approch.

1. The last/first element of post/pre Order array is the root element of the tree.Build the root Node. Find the root element in the InOrder array.
2. Count the number of elements of the left subarray and the right subarray of InOrder array, which is the number of elements in the corresponding left child subtree and right child subtree.
3. Divide the post/pre Order array into 3 part, the root, the right subarray contains a continuous elements of the number of right subarray of InOrder array, and the left vice versa.
4. Do the recurisve operation on the two subarrays.

- code:
    postOrder and InOrder to build tree
```c++
//根据后序和中序创建二叉树
//中序的首尾元素坐标，后序的首尾元素坐标
BinaryTreeNode<int>* createTree(int io1, int io2, int po1, int po2) {
    BinaryTreeNode<int> *p;  //新建一个结点(此处使用局部变量有风险)
    p=new BinaryTreeNode<int>(vPost[po2]);  //后序的最后一个元素是根结点

    //在中序里寻找根结点
    int i;
    for(i=io1; i<=io2; i++) {
        if (p->value() == vIn[i]){
            break;
        }
    }
    //左子树是中序从io1到io1+i-1， 后序从po1到po1+i-1
    //注意递归出口，只有当io1<=io1+i-1即i>=1时才有意义
    if (io1<=i-1){
        p->setLeftchild(createTree(io1, i-1, po1, po1+(i-1-io1)));
    }
    //右子树类似
    if (i+1<=io2){
        p->setRightchild(createTree(i+1, io2, po2-1-(io2-i-1), po2-1));
    }
    return p;
}
```

# MinHeap Implementation
property:
- AVL tree, can be stored in array
- parent k = i, left child k = 2i+1 ,right child k = 2i+2
- value of parent node is bigger than children nodes, no direct relation between cousin nodes

To Change to MaxHeap:
change method
- Insert,ShiftDown,ShiftUp,RemoveMax

```c++

template <class T>
class MinHeap  
{
private:
    T* heapArray;       //存放堆数据的数组
    int CurrentSize;    //当前堆中的元素数目
    int MaxSize;        //最大元素数目
public:
    MinHeap(const int n);   //构造函数，参数n为堆的最大元素数目
    virtual ~MinHeap() {delete []heapArray;};  //析构函数

    int parent(int pos) const;           //返回父结点位置

    bool RemoveMin(T& node);          //从堆顶删除最小值
    bool Insert(const T& newNode);   //向堆中插入新元素
    void SiftDown(int left);         //从left开始向下筛选  
    void SiftUp(int position);       //从position向上开始调整，使序列成为堆       
};

/*****************MinHeap类模板的实现************************/

template<class T>
MinHeap<T>::MinHeap(const int n)
{
    if(n<=0)
        return;
    CurrentSize=0;
    MaxSize=n;
    heapArray=new T[MaxSize];   //创建堆空间
}


template<class T>
int MinHeap<T>::parent(int pos) const   // 返回父节点位置 pos1/2 == (pos1 - 1)/2 ;pos2 - 1 == pos1
{   // return parent of both pos1 and pos2
    return (pos-1)/2;
}

template<class T>
void MinHeap<T>::SiftDown(int left)
{
    int i=left;                         //标识父结点
    int j=2*i+1;                        //标识关键值较小的子结点       
    T   temp=heapArray[i];              //保存父结点
    //过筛
    while(j<CurrentSize)
    {
        if((j<CurrentSize-1)&&(heapArray[j]>heapArray[j+1]))
            j++;                        //j指向右子结点
        if(temp>heapArray[j])
        {
            heapArray[i]=heapArray[j];
            i=j;
            j=2*j+1;
        }
        else break;
    }
    heapArray[i]=temp;
}

template<class T>
void MinHeap<T>::SiftUp(int position) 
{//从position向上开始调整，使序列成为堆
    int temppos=position;
    T temp=heapArray[temppos];
    while((temppos>0)&&(heapArray[parent(temppos)]>temp))
    {
        heapArray[temppos]=heapArray[parent(temppos)];
        temppos=parent(temppos);
    }
    heapArray[temppos]=temp;
}

template<class T>
bool MinHeap<T>::RemoveMin(T& node)
{
    if(CurrentSize==0){
        return false;
    }
    else{
        node = heapArray[0];
        heapArray[0]=heapArray[--CurrentSize];  //用最后的元素代替被删除的元素
        if (CurrentSize>1) {
            SiftDown(0);
        }
        return true;
    }
}

template<class T>
bool MinHeap<T>::Insert(const T& newNode)
{//向堆中插入一个结点
    if(CurrentSize>=MaxSize)
        return false;
    heapArray[CurrentSize]=newNode; 
    SiftUp(CurrentSize);    
    CurrentSize++;
    return true;
}
```

# BinarySearchTree Sort Implementation

BST Sort, InOrder traverse the BST created and the result is a sorted array.
BST inherited from BinaryTree. Key method is InsertNode which create the tree.

```c++
template <class T>
class BinarySearchTree:public BinaryTree<T> 
{
public:
    BinarySearchTree(){};
    virtual ~BinarySearchTree(){};
    void InsertNode(BinaryTreeNode<T>* newpointer);
    void DeleteNode(BinaryTreeNode<T>* pointer);
};

template <class T>
void BinarySearchTree<T>::InsertNode(BinaryTreeNode<T>* newpointer)
{
    BinaryTreeNode<T>* pointer=NULL; //初始化 
    if(root==NULL) {
        //用指针newpointer初始化二叉搜索树树根，赋值实现
        Initialize(newpointer);
        return;
    }
    else pointer=root;

    while(1)
    {
        if(newpointer->value()==pointer->value())
            return ;                                    //相等则不用插入
        else if(newpointer->value()<pointer->value())   //作为左子树
        {
            if(pointer->leftchild()==NULL)  {
                pointer->left=newpointer;
                return;
            }
            else    pointer=pointer->leftchild();
        }
        else{                                           //作为右子树
            if(pointer->rightchild()==NULL){
                pointer->right=newpointer;
                return;
            }
            else    pointer=pointer->rightchild();
        }
    }
}

template <class T>
void BinarySearchTree<T>::DeleteNode(BinaryTreeNode<T>* pointer)
{   
    if( pointer == NULL )   //如果带删除节点为空,返回
        return; 


    BinaryTreeNode<T> * parent = GetParent(root ,pointer ); //保存要删除节点的父节点 
    if( pointer->leftchild() == NULL )  //如果待删除节点的左子树为空,就将它的右子树代替它即可
    {       
        if( parent == NULL )                      //将右子树连到待删除节点的父的合适位置
            root = pointer->rightchild();
        else if( parent->leftchild() == pointer )
            parent->left = pointer->rightchild();
        else
            parent->right = pointer->rightchild();
        delete pointer;
        pointer=NULL;
        return;
    }

    //当待删除节点左子树不为空,就在左子树中寻找最大节点替换待删除节点
    BinaryTreeNode<T> * temppointer;  //保存要替换上来的节点 
    BinaryTreeNode<T> * tempparent = NULL;  //保存要替换上来的节点的父节点 
    temppointer = pointer->leftchild();     
    while(temppointer->rightchild() != NULL )
    {
        tempparent = temppointer;
        temppointer = temppointer->rightchild();
    }

    //删除替换结点
    if(tempparent==NULL)
        pointer->left=temppointer->leftchild();
    else
        tempparent->right=temppointer->leftchild();

    //用替换结点去替代真正的删除结点
    if(parent==NULL)
        root=temppointer;
    else if( parent->leftchild() == pointer )
        parent->left=temppointer;
    else parent->right=temppointer;

    temppointer->left=pointer->leftchild();
    temppointer->right=pointer->rightchild();

    delete pointer;
    pointer=NULL;
    return;
}
```