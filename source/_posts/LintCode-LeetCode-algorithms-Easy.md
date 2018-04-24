---
title: LintCode & LeetCode algorithms Easy
date: 2016-05-05 10:41:40
tags: Tips For Review
---
#### Merge Sorted Array II
Merge two given sorted integer array A and B into a new sorted integer array.
两数组从大到小排列合并算法     
Example
A=[1,2,3,4]      
B=[2,4,5,6]        
return [1,2,2,3,4,4,5,6]      
solution:   
// 因为数组有排序过 通过从数组尾到头 一个个比较 之后把大的元素插入新数组 实现  
```java
      public int[] mergeSortedArray(int[] A, int[] B) {
             int[] result = new int[A.length + B.length];
             int i = A.length - 1;
             int j = B.length - 1;
             int index = i + j + 1;
             while(j>=0 && i >= 0){
                 if (A[i] > B[j]){
                 result[index--] = A[i--];}else{
                     result[index--] = B[j--];
                 }
             }
             while(i>=0){
                 result[index--] = A[i--];
             }
             while(j>=0){
                 result[index--] = B[j--];
             }
             return result;
          }
```


#### First Position of Target
二分法查询实现：
For a given sorted array (ascending order) and a target number, find the first index of this number in O(log n) time complexity. If the target number does not exist in the array, return -1.     
```java
    public int binarySearch(int[] nums, int target) {
          if (nums == null || nums.length == 0) {
              return -1;
          }

          int start = 0, end = nums.length - 1;
          while (start + 1 < end) {  //这里由于求mid取整所以不会收缩到一个结果只会到最后两个结果 所以要＋1
              int mid = start + (end - start) / 2;
              if (nums[mid] < target) {
                  start = mid;
              } else {
                  end = mid;
              }
          }

          if (nums[start] == target) {
              return start;
          }

          if (nums[end] == target) {
              return end;
          }

          return -1;
      }
```

<!--more-->
#### Insert Interval
对象合并范围，用到arrayList，中心思想是通过比较范围，向新List里插入对象
Given a non-overlapping interval list which is sorted by start point.
Insert a new interval into it, make sure the list is still in order and non-overlapping (merge intervals if necessary).
```java
    public ArrayList<Interval> insert(ArrayList<Interval> intervals, Interval newInterval) {
        if (newInterval == null || intervals == null) {
            return intervals;
        }

        ArrayList<Interval> results = new ArrayList<Interval>();
        int insertPos = 0;

        for (Interval interval : intervals) {
            if (interval.end < newInterval.start) {
                results.add(interval);
                insertPos++;
            } else if (interval.start > newInterval.end) {
                results.add(interval);
            } else {
                newInterval.start = Math.min(interval.start, newInterval.start);
                newInterval.end = Math.max(interval.end, newInterval.end);
            }
        }

        results.add(insertPos, newInterval);

        return results;
    }
```

#### Merge Intervals

给一组区间，合并区间，办法是另建一个容器（arrayList）然后插入区间，这里有点小技巧
```java
    public List<Interval> merge(List<Interval> intervals) {
        ArrayList<Interval> result = new ArrayList<Interval>();
        for(int i = 0;i<intervals.size();i++){//遍历原来的列表 插入区间到结果列表
            insert(result,intervals.get(i));
        }
        return result;
    }
    List<Interval> insert(List<Interval> result, Interval newInterval){
        int index = 0;
        while(index < result.size()){遍历结果列表
            if(newInterval.end < result.get(index).start){//如果插入元素比当前index小则插入到当前index，返回
                result.add(index,newInterval);
                return result;
            }else if(newInterval.start >result.get(index).end){//如果比当前index大则到下一个区间比较
                index++;
                continue;
            }else{//如果区间有重叠则比较当前index和插入元素的开始结尾，保留大的边界，在结果列表删除当前index的元素，删除结果列表当前区间之后再循环，可以解决结果列表之后还有区间与插入区间重叠问题，
                newInterval.start = Math.min(newInterval.start,result.get(index).start);
                newInterval.end = Math.max(newInterval.end,result.get(index).end);
                result.remove(index);
            }
        }
        result.add(newInterval);//当index遍历完结果列表，加入到最后
        return result;
    }

```

#### Reverse Linked List
一个链表1->2->3->null 实现反转 3->2->1->null
方法是建立一个null链表 ，把目前head.next指向的对象先存下来，head.next指向新建链表，让新建列表头进一步到head，head进一步成为之前存下对象
```java
    public ListNode reverse(ListNode head) {
        ListNode prev = null;
        while (head != null) {
            ListNode temp = head.next;
            head.next = prev;
            prev = head;
            head = temp;
        }
        return prev;
    }
```

#### Recover Rotated Sorted Array
三步反转法对顺序轮转过的序列排序
三步反转法：对一个轮转过的顺序序列，找到序列中最大最小连接点的index，1.反转0到index子序列 2.反转index到列尾子序列 3.反转整个序列  即可得到顺序列  PS：此方法只对顺序轮转序列有效

Given a rotated sorted array, recover it to sorted array in-place.
```java
    private void reverse(ArrayList<Integer> nums, int start, int end) {//swap 交换函数
        for (int i = start, j = end; i < j; i++, j--) {
            int temp = nums.get(i);
            nums.set(i, nums.get(j));
            nums.set(j, temp);
        }
    }

    public void recoverRotatedSortedArray(ArrayList<Integer> nums) {
        for (int index = 0; index < nums.size() - 1; index++) {//三步反转法
            if (nums.get(index) > nums.get(index + 1)) {//找到index连接点
                reverse(nums, 0, index);     //1
                reverse(nums, index + 1, nums.size() - 1);     //2
                reverse(nums, 0, nums.size() - 1);     //3
                return;
            }
        }
    }
```

#### Maximum Subarray
动态规划求子数组合最大值
Given an array of integers, find a contiguous subarray which has the largest sum.
Example
Given the array [−2,2,−3,4,−1,2,1,−5,3], the contiguous subarray [4,−1,2,1] has the largest sum = 6.

###### // Version 1: Greedy
动态规划贪心：只要数组加合结果大于0就一直加，把加合最大值记下来
```java             
    public class Solution {
        public int maxSubArray(int[] A) {
        if (A == null || A.length == 0){
            return 0;
        }

        int max = Integer.MIN_VALUE, sum = 0;
        for (int i = 0; i < A.length; i++) {
            sum += A[i];//加合
            max = Math.max(max, sum);//记录最大最
            sum = Math.max(sum, 0);//加合大于0就继续加 否则从新开始新序列头开始加合
        }

        return max;
      }
    }
```

###### //version2:动规
    // LintCode, Maximum Subarray       
    // 时间复杂度 O(n),空间复杂度 O(1)                
```java
    class Solution {
      // 这个算法关键在于，之前加合的值如果为负则抛弃之前加合的子序列从当前开始，于此同时记录加合最大值        
        public:
        int maxSubArray(const vector<int> &A) {
            int result = INT_MIN, f = 0;
            for (int i = 0; i < A.size(); ++i) {
               f = max(f + A[i], A[i]);      //   比较目前值与之前加合值
               result = max(result, f);    // 记录最大值
            }
           return result;
        }
      };
```

#### Binary Tree Preorder Traversal
//二叉树先序遍历
前序遍历：根节点->左子树->右子树
中序遍历：左子树->根节点->右子树
后序遍历：左子树->右子树->根节点

先序遍历(Pre-Order Traversal)
指先访问根，然后访问孩子的遍历方式，其C代码如下
使用栈,时间复杂度 O(n),空间复杂度 O(n)
```java
          public List<Integer> preorderTraversal(TreeNode root) {//先访问的后入栈
              Stack<TreeNode> stack = new Stack<TreeNode>();
              List<Integer> preorder = new ArrayList<Integer>();

              if (root == null) {
                  return preorder;
              }

              stack.push(root);
              while (!stack.empty()) {
                  TreeNode node = stack.pop();
                  preorder.add(node.val);
                  if (node.right != null) {
                      stack.push(node.right);
                  }
                  if (node.left != null) {
                      stack.push(node.left);
                  }
              }

              return preorder;
          }
```
用迭代法实现
```java
          public ArrayList<Integer> preorderTraversal(TreeNode root) {
              ArrayList<Integer> result = new ArrayList<Integer>();
              traverse(root, result);
              return result;
          }
          // 把root为跟的preorder加入result里面
          private void traverse(TreeNode root, ArrayList<Integer> result) {
              if (root == null) {
                  return;
              }

              result.add(root.val);
              traverse(root.left, result);
              traverse(root.right, result);
          }
```

#### Binary Tree Inorder Traversal
//中序遍历 思想：先把左侧所有节点入栈直到目前节点为null，出栈节点并记录，当前节点去出栈节点的右节点，之后重复循环
中心思想：把一个节点的左节点搜索到头然后弹出并记录他 之后搜索他的右节点的左字节点 栈方法：        
```java
      public ArrayList<Integer> inorderTraversal(TreeNode root) {
              Stack<TreeNode> stack = new Stack<TreeNode>();
              ArrayList<Integer> result = new ArrayList<Integer>();
              TreeNode curt = root;
              while (curt != null || !stack.empty()) {
                  while (curt != null) {
                      stack.push(curt);
                      curt = curt.left;
                  }
                  curt = stack.pop();
                  result.add(curt.val);
                  curt = curt.right;
              }
              return result;
          }
```
递归循环方法：
```java
    public ArrayList<Integer> inorderTraversal(TreeNode root) {
              // Stack<TreeNode> stack = new Stack<TreeNode>();
              ArrayList<Integer> result = new ArrayList<Integer>();
              traverse(root,result);
              return result;
          }
          public void traverse(TreeNode root,ArrayList<Integer> result){
              if(root == null){
                  return;
              }

              traverse(root.left,result);
              result.add(root.val);
              traverse(root.right,result);
          }
```

#### Binary Tree Postorder Traversal
//迭代循环方法
```java
    public ArrayList<Integer> postorderTraversal(TreeNode root) {
          ArrayList<Integer> result = new ArrayList<Integer>();
          Stack<TreeNode> stack = new Stack<TreeNode>();
          TreeNode prev = null; // previously traversed node
          TreeNode curr = root;

          if (root == null) {
              return result;
          }

          stack.push(root);
          while (!stack.empty()) {
              curr = stack.peek();
              if (prev == null || prev.left == curr || prev.right == curr) { // traverse down the tree
                  if (curr.left != null) {
                      stack.push(curr.left);
                  } else if (curr.right != null) {
                      stack.push(curr.right);
                  }
              } else if (curr.left == prev) { // traverse up the tree from the left
                  if (curr.right != null) {
                      stack.push(curr.right);
                  }
              } else { // traverse up the tree from the right
                  result.add(curr.val);
                  stack.pop();
              }
              prev = curr;
          }

          return result;
      }
```
//递归方法
```java
    public ArrayList<Integer> postorderTraversal(TreeNode root) {
              ArrayList<Integer> result = new ArrayList<Integer>();
              traverse(root,result);
              return result;
          }

          public void traverse(TreeNode root, ArrayList<Integer> result){
              if (root == null){
                  return;
              }

              traverse(root.left,result);
              traverse(root.right,result);
              result.add(root.val);

          }
```

#### Single Number
//一个2n＋1长度数组，除了一个元素出现一次其他元素出现两次，找出现一次的元素
用亦或运算累加，出现两次的全部加为0
```java
          public int singleNumber(int[] A) {
              if(A == null || A.length == 0) {
                  return 0;
              }
              int rst = 0;
              for (int i = 0; i < A.length; i++) {
                  rst ^= A[i];
              }
              return rst;
          }
```

#### Insert Node in a Binary Search Tree
//中序遍历顺序的树，现在给出一个节点让放入正确位置。

        example:
          2             2
         / \           / \
        1   4   -->   1   4
           /             / \
          3             3   6
想法是看这个节点的值，如果大于父节点则在右枝否则在左枝，然后用递归循环
```java
          public TreeNode insertNode(TreeNode root, TreeNode node) {
              if (root == null) { //这里很重要 循环到没有节点时返回目标节点赋值
                  return node;
              }
              if (root.val > node.val) {//判断向左还是向右
                  root.left = insertNode(root.left, node);//递归 注意函数返回值是新的父节点所以可以这么赋值 当递归到最后 即当前父节点的左右均为null 则这里赋值到左右。
              } else {
                  root.right = insertNode(root.right, node);
              }
              return root;// 返回新的父节点
          }
```

#### Partition List
Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x. You should preserve the original relative order of the nodes in each of the two partitions.         
//给一个数，使一个链表中大于等于这个数字的元素排后面，小于这个元素的排前面且不改变原始的相对位置。方法为新设两个链表和两个指针，遍历原链表，小的放在小链表大的放在大链表然后把大链表接在小链表后面。
```java
          public ListNode partition(ListNode head, int x) {
             if (head == null){
                 return null;
             }

             ListNode leftStart = new ListNode(-1);
             ListNode rightEnd = new ListNode(-1);
             ListNode left = leftStart;
             ListNode right = rightEnd;

             while(head != null){
                 if (head.val < x){
                     left.next = head;
                     left = left.next;
                 }else{
                     right.next = head;
                     right = right.next;
                 }
                 head = head.next;
             }

             right.next = null;
             left.next = rightEnd.next;
             return leftStart.next;
          }
```

#### Maximum Depth of Binary Tree
//用递归法主要归纳递归逻辑 开始状态 过程状态 和结果状态
开始是用左右两边的最大深度加1（自己）返回结果
结束状态是到最下层返回0到上层
```java
          public int maxDepth(TreeNode root) {
              if (root == null){
                  return 0;
              }
              return Math.max(maxDepth(root.left),maxDepth(root.right)) + 1;
          }
```

#### Remove Duplicates from Sorted Array
//从排序的数组中去除出现过一次以上的元素，由于数组不能修改大小，我们把前m项作为缩减之后的数组
example:Given input array A = [1,1,2] Your function should return length = 2, and A is now [1,2].
用双指针，一个指当前元素，一个遍历数组，遇到不同的则当前元素下一个元素被赋值为不同的值
```java
      public int removeDuplicates(int[] A) {
              if (A == null || A.length == 0) {
                  return 0;
              }

              int size = 0;
              for (int i = 0; i < A.length; i++) {
                  if (A[i] != A[size]) {
                      A[++size] = A[i];//相同则i继续遍历数组，不同则size自增然后赋值
                  }
              }
              return size + 1;
          }
```

#### Remove Duplicates from Sorted Array II
//类似上一题，不过这次一个元素可以出现最多两次
example:Given sorted array A = [1,1,1,2,2,3] Your function should return length = 5, and A is now [1,1,2,2,3].
```java
          public int removeDuplicates(int[] nums) {
              if (nums == null || nums.length == 0){
                  return 0;
              }
              int index = 0;
              int c =0;
              int size = 0;
              while(index < nums.length){
                  if(nums[size] == nums[index] ){
                      if(c==1){
                          nums[++size] = nums[index];
                      }
                      c++;
                  }else{
                      nums[++size] = nums[index];
                      c = 1;
                  }
                  index++;
              }
              return ++size;
          }
```

#### Triangle
//给一个n＊n的矩阵其中对角线及左侧的三角为triangle求从最底层triangle[n-1][*]到triangle[0][0]的合最小值。
这里如果用贪心算法可能得不到最优解，还是要全部遍历。转移方程为：
f(i,j) = min{f(i+1,j),f(i+1,j+1)} + d(i,j);//最小值线路是之前最小值加上下一个路径中最小一个点
由于一共有n个可能线路，所以从下往上遍历计算比较方便。
```java
      public int minimumTotal(int[][] triangle) {
          if (triangle.length == 0 || triangle[0].length == 0){
              return -1;
          }
          for(int i = triangle.length - 1;i>=1;i--){ //从最低一行往上遍历
              for(int j = 0 ; j < i ; j++){ //每一行遍历每一个元素等于他下面两个元素最小路径加他本身

               triangle[i - 1][j] += Math.min(triangle[i][j],triangle[i][j+1]);
              }
          }
          return triangle[0][0];
      }
```

#### Minimum Path Sum
//给一个m*n的矩阵，矩阵每一个元素有非零值，求从矩阵左上到右下走的通路中花费最小的值。
动态规划：转移方程为f(i,j) = min{f(i-1,j),f(i,j-1)}+d(i,j);
f(i,j)为到（i,j）点所需要的费用，等于他左边点与上边点中最小费用加上（i，j）点本身的费用。
//version1:动态规划
建立一个新矩阵，把最左边和最上边的行用原矩阵加合填入。之后遍历中间点，每一个点值由转移方程得出。
```java
          public int minPathSum(int[][] grid) {
              if (grid == null || grid.length == 0 || grid[0].length == 0) {
                  return 0;
              }

              int M = grid.length;
              int N = grid[0].length;
              int[][] sum = new int[M][N];

              sum[0][0] = grid[0][0];

              for (int i = 1; i < M; i++) { //最左边列
                  sum[i][0] = sum[i - 1][0] + grid[i][0];
              }

              for (int i = 1; i < N; i++) { //最上边行
                  sum[0][i] = sum[0][i - 1] + grid[0][i];
              }

              for (int i = 1; i < M; i++) {
                  for (int j = 1; j < N; j++) { //根据转移方程，遍历填入中心点
                      sum[i][j] = Math.min(sum[i - 1][j], sum[i][j - 1]) + grid[i][j];
                  }
              }
              return sum[M - 1][N - 1]; //矩阵右下点为最后结果
          }
```

#### Climbing Stairs
//n阶楼梯，每次可以上一次或两次，求到n有多少种走法

    Example：
         Given an example n=3 , 1+1+1=2+1=1+2=3
         return 3
用动态规划的思想，第n阶楼梯的走法是由第n-1阶走法加上n-2阶楼梯走法构成，状态方程：
     f(n) = f(n-1)+f(n-2);//为fibonacci递推
第一阶有1种走法，第二阶有两种走法：
```java
          public int climbStairs(int n) {//fibonacci 题的变形
              if(n==0){
                  return 1;
              }
              int i = 2;
              int[] stair = new int[n];
              stair[0] = 1; //初始化
              if(n > 1){
                  stair[1] = 2;
                  while(i < n ){ //收敛条件
                      stair[i] = stair[i-1] + stair[i-2];
                      i++;
                  }
              }
              return n > 1 ? stair[n-1] : stair[0];
          }
```

#### Unique Paths
//一个m*n的矩阵，从左上角走到右下角，只能向左或向下，求不同走法的数量   
##### version1:基本算法，动态规划                 
转移函数：
               f(i,j) = f(i-1,j)+f(i,j-1);//走到(i,j)点的方法为走到(i-1,j)和(i,j-1)点方法的和。           

```java
              public int uniquePaths(int m, int n) {
                  if( m == 0 || n == 0 ||m == 1||n==1){
                      return 1;
                  }
                  int[][] f = new int[m][n];
                  for(int i =0;i<m;i++){//把左边首列填满1
                      f[i][0] = 1;
                  }
                  for(int j = 0;j<n;j++){//把顶行填满1，初始化
                      f[0][j] = 1;
                  }
                  for(int i = 1;i<m;i++){
                      for(int j = 1;j<n;j++){
                          f[i][j] = f[i-1][j] + f[i][j-1];//转移方程
                      }
                  }
                  return f[m-1][n-1];
              }
```

##### version2:动态规划滚动算法。                      
利用小技巧，把每一次上门一层线加到下面一层，然后在由左向右加
```java
              public int uniquePaths(int m, int n) {
               int[] f = new int[n];
                    f[0] = 1;
                  for(int i = 0;i<m;i++){
                      for(int j = 1;j<n;j++){
                          f[j] = f[j-1] + f[j];//转移方程
                      }
                  }
                  return f[n-1];
              }
```

##### version3:DFS搜索加缓存                            
用DFS迭代搜索，为了减少内存分配，减少不必要运算，线创建一个f[m][n]的矩阵，然后运算，在getOrUpdate()函数中检查，如果元素之前运算过则直接返还之前的值，如果元素之前没运算过（为零）则进行迭代运算，并把结果纪录下来。            
```java
          public int uniquePaths(int m, int n) {
               this.f = new int[m+1][n+1];
               }

          int[][] f; // 缓存

          int dfs(int x, int y) {
               if(x<1||y<1)return0;// 数据非法,终止条件
               if (x == 1 && y == 1) return 1; // 回到起点,收敛条件
               return getOrUpdate(x - 1, y) + getOrUpdate(x, y - 1);
                }

          int getOrUpdate(int x, int y) { //之前有运算过返还值，否则迭代运算并且储存结果。
                    if (f[x][y] > 0) return f[x][y];
                    else return f[x][y] = dfs(x, y);

               }
```

#### Unique Paths II          
//在之前Unique Paths 基础上，给你一个矩阵，里面有1的元素表示墙，走不通 还是用之前的动态规划算法，不过这里的缓存矩阵f[][]每当原来矩阵里有1时，f矩阵里要设为0表示没有通路到这个(i,j)        
```java
              public int uniquePathsWithObstacles(int[][] obstacleGrid) {
                  final int m = obstacleGrid.length;
                  final int n = obstacleGrid[0].length;
                  int[][] f= new int[m][n];

                  for(int i = 0;i<m;i++){//初始化首行 遇到1则设为零，后面值等于前一个值
                      f[i][0] = obstacleGrid[i][0] == 1 ? 0 : (i==0?1:f[i-1][0]);
                  }
                  for(int j = 0;j<n;j++){//初始化列
                      f[0][j] = obstacleGrid[0][j] == 1 ? 0 : (j==0?1:f[0][j-1]);
                  }
                  for(int i =1;i<m;i++){
                      for(int j = 1;j<n;j++){//转移方程，当原来矩阵里为1，f矩阵为0，表示到这点路径为0；
                          f[i][j] = obstacleGrid[i][j] == 1 ? 0 :f[i-1][j] +f[i][j-1];
                      }
                  }
                  return f[m-1][n-1];
              }
```

#### Sqrt(x)          
//实现平方根用逼近函数          
```java
          public int sqrt(int x) {
              if (x <= 0){
                  return 0;
              }
              long g = 1;
              while(true){
                   if((g+1)*(g+1) >= x && g*g <= x){
                      return (int)g;
                  }
                  g = (g+x/g)/2; //逼近函数，
              }
            }
```



#### Merge Two Sorted Lists         
//合并两个排序的链表，不用去除重复的元素。使用双指针法，把两个链表拼接起来，不用新建另一个链表
```java
             public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
                 if(l1 == null){
                     return l2;
                 }else if(l2 == null){
                     return l1;
                 }
                 ListNode result = new ListNode(0);
                 ListNode index = result;
                  while(l2 != null && l1 != null){
                      if (l2.val < l1.val){ //连个指针依次向后移动，根据大小拼接链表
                          index.next = l2;
                          l2 = l2.next;
                      }else{
                          index.next = l1;
                          l1 = l1.next;
                      }
                      index = index.next;
                  }
                  if(l1 == null){
                      index.next = l2;
                  }else{
                      index.next = l1;
                  }
                 return result.next;
              }
```

#### Nth to Last Node in List         
//求一个链表中某Node，其到最后（null）有n步；做法位双指针，两指针之前差n步，一起向后移动直到front元素为null        
例子：3->2->1->5->null and n = 2，return Node with Node.val == 1;     
```java
              ListNode nthToLast(ListNode head, int n) {
                  ListNode back = head,front = head;
                  int i = 0;
                  while( i < n){
                          if(front != null){
                          front = front.next;
                          i++;
                          }else{
                              return null;
                           }
                  }
                  while(front != null){

                      front = front.next;
                      back = back.next;

                  }
                  return back;
              }
```

#### Add Binary       
//给两个字符串代表的二进制数，求做二进制加法之后的二进制数组的字符串表示；这里用二进制按位加算法外加字符操作     
```java
              public String addBinary(String a, String b) {
                  String result = "";
                  final int n = a.length() > b.length() ? a.length():b.length();
                  a = new StringBuffer(a).reverse().toString();   //为了方便从低位开始遍历所以反转
                  b = new StringBuffer(b).reverse().toString();
                  int carry = 0;
                  for(int i = 0;i<n;i++){
                      int ai = i < a.length() ? a.charAt(i) - '0': 0;//ai本来是char 为了得到ai位的数字方便计算
                      int bi = i < b.length() ? b.charAt(i) - '0' : 0;
                      int val = (ai + bi + carry) % 2;     //算位值
                      carry = (ai + bi + carry ) / 2;      //算进位
                      result = new StringBuilder(result).insert(0,(char)(val+'0')).toString();//val加上‘0’ 在asc2中偏移 48 以便得到‘0’或‘1’；

                  }
                  if(carry == 1){ //如果有进位剩下，则进位
                      result = new StringBuilder(result).insert(0,'1').toString();
                  }
                  return result;
              }
```

#### Add Two Numbers            
//类似上一题，给两个链表包含反转过的整数 求这两个数的和 按位反转存入另一个链表 ，由于当给的数据太大没有数据类型可以装下时，用这张按位加减的算法            
```java
              public ListNode addLists(ListNode l1, ListNode l2) {
                  ListNode result = new ListNode(0);
                  ListNode index = result;
                  int val = 0 , carry = 0;
                  for(;l1 != null || l2 != null ; l1 = l1 == null ? null : l1.next , l2 = l2 ==  null ? null : l2.next,index = index.next){
                      int i1 = l1 != null? l1.val : 0;     //看是已经按位加到头，是则返回零
                      int i2 = l2 != null? l2.val : 0;
                      val = (i1+i2+carry) % 10;     //位值
                      carry = (i1 + i2 + carry) / 10;     //进位
                      index.next = new ListNode(val);
                  }
                  if(carry > 0){
                      index.next = new ListNode(carry);
                  }
                  return result.next;
              }
```

#### Remove Element     
//给一个数组给一个数，把与这个数相同的数移动到数组最后，返回数组前段不包含这个数到子数组的大小                   
```java
              public int removeElement(int[] A, int elem) {
                  int i = 0;
                  int pointer = A.length - 1;
                  while(i <= pointer){
                      if(A[i] == elem){
                          A[i] = A[pointer];
                          pointer--;
                      } else {
                          i++;
                      }
                  }
                  return pointer + 1;
              }
```

#### Insertion Sort List          
//插入排序的链表实现，重要；中心思想是用node.next操作，找到node.next.val >head.val 或 node.next == null的点然后插入head的节点           
```java
public ListNode insertionSortList(ListNode head) {
  ListNode dummy = new ListNode(0);
  // 这个dummy的作用是，把head开头的链表一个个的插入到dummy开头的链表里
  // 所以这里不需要dummy.next = head;

  while (head != null) {
      ListNode node = dummy;
      while (node.next != null && node.next.val < head.val) {
          node = node.next;
      }
      ListNode temp = head.next;//保存head之后的链
      head.next = node.next;//把head连接上之后到链
      node.next = head;//head接到node链里；连着2步把head节点插入到node.next上
      head = temp;//head ＝ head.next；
  }

  return dummy.next;
}
```

##### Remove Nth Node From End of List
//从链表中删除倒数第N个节点，在开头接一个dummy节点，找到倒数第N个点的前一个，删除第N个
```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    if (n <= 0) {
        return null;
    }

    ListNode dummy = new ListNode(0);
    dummy.next = head;

    ListNode preDelete = dummy;
    for (int i = 0; i < n; i++) {
        if (head == null) {
            return null;
        }
        head = head.next;
    }
    while (head != null) {
        head = head.next;
        preDelete = preDelete.next;
    }
    preDelete.next = preDelete.next.next;
    return dummy.next;
}
```

##### Invert Binary Tree
//给一个二叉树，把二叉树的所有字节点互换，左变右，右变左；就是二叉树遍历，遍历同时操作就行
这里用栈方法遍历二叉树，前面有所有二叉树遍历方法
```java
public void invertBinaryTree(TreeNode root) {//stack method
   if (root == null) {
       return;
   }

   Stack<TreeNode> stack = new Stack<TreeNode>();

   stack.push(root);
   while(!stack.empty()){
       TreeNode curt = stack.pop();
       TreeNode temp = curt.left;
       curt.left = curt.right;
       curt.right = temp;

        if(curt.right != null){
           stack.push(curt.right);
        }
        if(curt.left != null){
           stack.push(curt.left);
        }

   }
 }
```

##### Balanced Binary Tree
//检查一个binary tree 是不是 AVL tree，是否平衡；根据AVL property 需要每一个node的两个子node高度之差不大于1
用迭代方法：当前的node符合的条件是 1.两个子node符合property 2.这两个子node高度差不大于1.
```java
  public boolean isBalanced(TreeNode root) {
        if(root == null){
            return true;
        }
        boolean left = isBalanced(root.left);
        boolean right = isBalanced(root.right);
        if(left && right && Math.abs(maxLength(root.left) - maxLength(root.right))<=1){
            return true;
        }
       return false;
    }
    public int maxLength(TreeNode root){ //计算node高度function
        if(root == null){
            return -1;
        }
        int left = maxLength(root.left);
        int right = maxLength(root.right);
        return Math.max(left,right) + 1;
    }
```

##### Convert Sorted Array to Binary Search Tree With Minimal Height
//用排过序的数组创建AVL tree
一种方法：类似binary search ，把每一个数组中位数当root 然后把root左右当作2个新数组 迭代实现
```java
    private TreeNode buildTree(int[] num, int start, int end) {
        if (start > end) {
            return null;
        }

        TreeNode node = new TreeNode(num[(start + end) / 2]);
        node.left = buildTree(num, start, (start + end) / 2 - 1);
        node.right = buildTree(num, (start + end) / 2 + 1, end);
        return node;
    }

    public TreeNode sortedArrayToBST(int[] num) {
        if (num == null) {
            return null;
        }
        return buildTree(num, 0, num.length - 1);
    }
```

##### Plus One
//给一个数组用来表示一个大数，给这个大数加一然后返回装这个大数的数组
由于数字太大的话没办法用数据类型装下，一个一个位操作，如果有进位则新建一个数组把数据填入
```java
    public int[] plusOne(int[] digits) {
        int carries = 1;
        for(int i = digits.length-1; i>=0 && carries > 0; i--){  // fast break when carries equals zero
            int sum = digits[i] + carries;
            digits[i] = sum % 10;
            carries = sum / 10;
        }
        if(carries == 0)
            return digits;

        int[] rst = new int[digits.length+1];
        rst[0] = 1;
        for(int i=1; i< rst.length; i++){
            rst[i] = digits[i-1];
        }
        return rst;
    }
```

##### Reverse Integer
//给一个int数字输出反转int数字，如果反转十进制数字溢出则返回0
Example：
Given x = 123, return 321

```java
    public int reverseInteger(int n) {
        int reversed_n = 0;

        while (n != 0) {
            int temp = reversed_n * 10 + n % 10; //一位一位反转
            n = n / 10;
            if (temp / 10 != reversed_n) { //当到最后一位，如果溢出则条件不成立
                reversed_n = 0;
                break;
            }
            reversed_n = temp;
        }
        return reversed_n;
    }
```

##### Count and Say
//似乎是facebook面试题。数组 1, 11, 21, 1211, 111221； 生成方式：第一个为1，第二个为数且说前一个，为1个1即11、第三个为2个1即21，第四个为一个2一个1即1211，第五个为一个1一个2两个1即111221，以此类推。求此数组第n个元素。
直接翻译成代码，需要从1开始n次迭代，每一个迭代时从上市次得到的字符串中计算，数一样的元素数量，加上元素本身数值附在临时数组尾，到第n次得到需要的值，转换成字符串。
```java
    public String countAndSay(int n) {
        String oldString = "1";
        //从1到n次迭代
        while (--n > 0) {
            StringBuilder sb = new StringBuilder();
            char [] oldChars = oldString.toCharArray();

            for (int i = 0; i < oldChars.length; i++) {
                int count = 1;
                //每一位i的下一位i＋1不大于字符串长度，且i位与i＋1位值一样
                while ((i+1) < oldChars.length && oldChars[i] == oldChars[i+1])
                {   //纪录一样的值数目，且指针前进
                    count++;
                    i++;
                }
                //迭代一次，在临时字符串尾加上 1某个数字出现数目 2数字的值
                sb.append(String.valueOf(count) + String.valueOf(oldChars[i]));
            }

            oldString = sb.toString();
        }

        return oldString;
    }
```

##### Length of Last Word
求给定字符串最后一个单词的长度，直接用指针算出长度即可，要注意字符串最后一个字符有可能不是字母。
使用Character单例函数判断是否是字母或者空格。
```java
    public int lengthOfLastWord(String s) {
        int index = s.length() - 1,result = 0;

        while(index >= 0){
            char temp = s.charAt(index);
            if (!Character.isLetter(temp) && result == 0){
                index--;

            }else
            if ( Character.isLetter(temp)){
                index--;
                result++;
            }else
            if (Character.isWhitespace(temp)){
                break;
            }

        }
        return result;
    }
```

##### Number of Islands
给一个2维布尔数组，为1以及它相邻的1定义为一个岛，为0及相邻的0定义为海，求岛数目。（这里的相邻仅仅为上下左右）
解法为要是扫描数组，遇到1则：1、把这个元素归零，结果增加一。2、用迭代找它相邻元素，重复1。
```java
    public int numIslands(boolean[][] grid) {
        int result = 0;
        for(int i = grid.length - 1;i >= 0;i--){
            for (int j = grid[0].length - 1;j >= 0;j--){
                if(grid[i][j] == true){
                    result++;
                    blankIt(grid,i,j);
                }
            }
        }
        return result;
    }

    void blankIt(boolean[][] grid,int x,int y){
        int rowMax = grid.length - 1;
        int coloumMax = grid[0].length - 1;
        grid[x][y] = false;
        if(x + 1 <= rowMax && grid[x + 1][y] == true){
            blankIt(grid,x + 1,y);
        }
        if(x - 1 >= 0 && grid[x - 1][y] == true){
            blankIt(grid,x - 1,y);
        }
        if(y - 1 >= 0 && grid[x][y - 1] == true){
            blankIt(grid,x,y - 1);
        }
        if(y + 1 <= coloumMax && grid[x][y + 1] == true){
            blankIt(grid,x,y + 1);
        }
    }
```

##### Ugly Number
只有2、3、5因子的正整数数被认为是丑数，比如6，8，10. 其它有其它因子的不是丑数，如14，包含因子7.
实现方法是看用2、3、5尽除完一个数后其为不为1。
```java
    public boolean isUgly(int num) {
        if (num <= 0) return false;
        if (num == 1) return true;

        while (num >= 2 && num % 2 == 0) num /= 2;
        while (num >= 3 && num % 3 == 0) num /= 3;
        while (num >= 5 && num % 5 == 0) num /= 5;

        return num == 1;
    }
```
