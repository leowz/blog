---
title: MIT6.006_DataStructureAndAlgorithms
date: 2016-06-05 00:58:28
tags:
---
#### lecture 2:
Document  distance problem : d(D1,D2)?      
-document = sequence of words.        
-word = string of chars.          
distance : look at the shared words

#### lecture3:binary search

#### 4.Heap and heap sort

Heap also called priority queue , its the implementation of a priority queue,and by putting the elements of this queue into a tree we have heap, the relation of array index in the tree is as follow:
     1.root :first element
     2.parent(i)  = floor(i/2);//index of i`s parent
     3.left(i) = 2i + 1; right(i) = 2i;     //index of i`s children

heap property:
- Max heap: for every node i : `A[parent(i)] >= A[i] ;`
- Min heap: for every node i: `A[parent(i)] <= A[i] ;`

##### Max_heapify(A,i):       
Assume that the tree rooted at left(i) and right(i) are maxheap check the value of element i and left(i) right(i) exchange the largest with element i

##### max Heap:        
the key of a node is >= the keys of its children
build max-Heap(A):

          for i =n/2 down to 1      
          //here [n/2 - n] is all leaves as is tree`s property
          do max_heapify(A,i);

##### heap sort:    
1.build max_heap from unordered array       
2.find A[i] which is the top element          
3.swap element A[n] with A[1] and the max element is at the end of the array          
4.discard node A[n] from the heap by decrementing heap size             
5.new root may violate max property but its children are max heap so run max_heapify the tree         
6.goto step 2
<!--more-->
#### 5.Binary Search Tree:BST in order by building the tree from array      
##### PROPERTY:
For all node x, if y is in the left subtree of x then val(y) <= val(x);         
If y is in the right subtree of x then val(y) >= val(x);

##### INSERTION with check: o(h) time with h equals to the hight of the tree
Operations:         
     find-min():go to the left with o(h);                 
     find-max():go to the right with o(h);                             


5.recitation:Augment the BTS structure:              
data structure: a bunch of algorithms that help you store and retrieve information          

queries : MAX,Min,Next-larger,Search  //ask questions to the data structure and it dose not change    
updates: Insert,Delete  

check RI(representation invariant):check the entire data structure and make sure the RI holds, if it dose not it raise an exception                                                      

Next-Larger:given a node find the next node which is just larger then this node but smaller then any other node                               
1.go to the right node of the given node.       
2.go all the way left in this sub tree to find the smallest                     
3.if the given node does not have a right sub tree then the next-larger will be:      
if given node is the left child of his parent then his parent node  will be               
if given node is the right child then no next-larger          

Deletion:to delete one node ,first the the next-larger of this node, then replace the node you want to delete with the next-larger node. //1.set the value of node to delete to the value of the next larger, 2. delete the next larger.

#### lecture 6. ALV tree,ALV sort
hight of BST:length of the longest path,in a balanced tree o(log n);        
hight of a node:length of the longest path from it down to a leaf;                
Compute the length of a node : Max(length(node.left),length(node.right)) + 1;

AVL Trees       
AVL property：require hight of left and right children of every node to differ by at most 1;

Nh = min. #nodes in an AVL tree of hight h;                      
Nh = 1 + Nh-1(number of nodes in the right sub tree)+ Nh-2( number of nodes in the left sub tree)                                   
Nh = 1 + Nh-1 + Nh-2 >1 + 2*Nh-2 > 2*Nh-2 = o(2^(h/2))     
so => h < 2 *lgN and the exact result is 1.44 * lgN calculated by the fibonacci series bound

AVL Insertion

- simple  BST insertion
- fix AVL property from change node up

- suppose x is lowest node violating AVL
- assume x.right higher
- if x`s right child is right-heavy or balanced right rotate x
- else if right child is left-heavy right rotate x`s right child ad left rotate x;

Rotation:           
left rotate:roof goes to the left           
right rotate:roof goes to the right


AVL sort:

-   do AVL insert to insert the n items of the array which needs to be sorted; o(n*log n )
-   do inorder traverse to get the sorted array;   ;o(n)

AVL rep invariant:
- BST
- for any node  | hr - hl|<=1;

#### lecture : 7
- comparison model
- lower bounds
     - searching: o(lgn)
     - sorting : o(n * lgn)
- o(n) sorting algorithms
     - counting sort
     - radix sort

Comparison Model
- all input items are black boxes (abstract data structure )
- only operation allowed are comparisons
- time cost = # comparison

Decision Tree         
     any comparison algorithm can be viewed as a tree of all possible comparisons and their outcomes and resulting answers,for any particular n item                                  
e.g. binary search algorithm with n =3

decision tree     |     algorithm
--------        |-------
internal node    |    binary decisions
leaf              |    found answer
root to leaf      | algo execution
length of path  | running time
the hight of the tree | worst running time

Searching lower bound                                   
 n preprocessed items , finding a given item among them in comparison model require  o(lgn) in worst case,for lgn
is the tree which have the lowest hight and maximum nodes when you look at the decision tree.

Sorting lower bound       
  number of  leaves >= number of  possible answers n!         
     hight >= n! >= n *lgn

Counting Sort : o(n+k)
1. initialize an array with length the maximum value +1 of the ints you want to sort
2. iterate all the values of the ints increment the array with the subscription identical to the values of the ints
3. output the values of the array,output value is the subscription of the array and number of value equals to the value of the specific value of

       L:array of K empty lists
       for i in range(n);
            L(key(A[j])).append(A[j]);
       outPut = [];
       for i in range(K);
            outPut.extend(L[i]);

counting sort:with position array

     sum = k // length of the original array which is the ints to be sorted;
     for i =k-1 to 0
          pos[i] = sum - pos[i];//in place changing from the counting array got above to the position array , start with pass0
          sum = pos[i]       //end with pass1
     out = [  ];
     for i = 0 to k-1
          out[pos[a[i]]] = a[i]; // a[i] is the array needs to be sorted ,basically a[i] value refers to pos`s index and pos`s value refer to out[]`s index
          pos[a[i]] = pos[a[i]] + 1; //by incrementing pos[]

property stable: counting sort will items to be sorted the same way as they are unsorted if the items are identical, it does not break the relative relationship  between items if they are the same. stable algorithm is important because in some context,some complex obj will be mapped to numbers and maybe two different obj have the same number. So we want to keep the relative order of the object the same after sorting, if they are mapped to the same numbers.

Radix Sort: d rounds of counting sort             
- imagine each integer as  base b
- #digits = d = logb K
- sort ints by  the least significant digit using counting sort
…...
- sort  by the least significant digit  to the most significant digit using counting sort
total time : o(n* logn K);

##### lecture 8 Hashing
- Dictionary
- Motivation
- Prehashing
- Hashing
- Chaining
- Simple Uniforme Hashing
- Good Hash Function

###### Dictionary : abstract data type
maintain a set of items with each item mapped with a key
- `insert(item);` :will overwrite any existing key
- `delete(item);`
- `search(key);` : return the item of the given key or report error

###### python: dict
D[key] ~ search         
D[key] = val ~ insert           
dele D[key] ~ deletion          

item = ( key, val )

###### Motivation
- docDict
- database
- interpreter

###### simple approach:
Direct - access table:store items in an array, index by key

###### prehasing
- map keys to none negative integers

###### hasing
- reduce universe of all keys down to a small set of integers
idea :  space m = o(n) ; m is the size of hash table and n is number of keys
collision : h(ki) = h(kj) while ki != kj

###### chaining
linked list of colliding elements in each slot of hash table

###### hash functions

- division method: hash(key)  = key mod m ; m is the size of your hash table
- multiplication: hash(key) = [(a*key) mod 2^w]>> ( w - r ); w is the number of bits of key which means key has w bits , a is a random integer; m = 2^r;
- universal hashing:  hash(key) = [(a*key + b) mod p] mod m; a and b are random numbers {0 …. p-1} ,p is the prime number which is bigger than the universe

##### lecture 9 Table Doubling, Karp-Rabin
- Rolling hash:
is a hash function where the input is hashed in a window that moves through the input. One of the main application is Rabin-Karp String search algorithm.
     - r.append(c) : add char c to the end of x
     - r.skip(c) : delete first char of x
     - r() :  hash value of x r() = h(x);
ps: through the operation above,  we can move the sliding window of rolling hash through out the text we want to hash and get the hash result fast.

- Karp - Rabin algorithm
using hash function in string matching , using rolling hash to quickly compute the hash of strings to match, if the target and the sample`s hash results matches then check the two strings is the same or not, if not continue otherwise return result.
```python
function RabinKarp(string s[1..n], string pattern[1..m])
  hpattern := hash(pattern[1..m]);  hs := hash(s[1..m]) //预处理两个hash值
  for i from 1 to n-m+1
    if hs = hpattern
      if s[i..i+m-1] = pattern[1..m] // 如果hash值相同且字符串相同则返回结果
        return i
    hs := hash(s[i+1..i+m]) //否则rolling hash到下一个字节
  return not found
```

#### lecture 10 Open Addressing cryptographic Hashing
- Open Addressing
- Uniform hashing analysis
- Cryptographic hashing

##### Open Addressing
no chaining to deal with collision
M ( slot in the hash table using array) N( numbers of elements)
M >= N
- Probing

meaning when do hashing trying to insert each item to the table,if it fails which means the given key`s item array is full, then recompute the hash using a different hash function.

Hash function specifies order of slots to probe a key. hash will take U(universe of keys) and C(trial count) to produce keys{1- m-1}

arbitrary K h(k,1),h(k,2)…….h(k,m-1) to be the permutation of 0 , 1 , 2 ,….m-1 which is the slot in the hash table

- Insert(k,v)

     keep probing until an empty slot is founded including delete me flag and none,insert item when found

- Search(k)

     As long as the slots probing encountered are occupied by keys != k ,keep probing until encounter k or find an empty slot which means fails to find k in the keys. If encountered delete me keep on probing.

- Delete(k)

     As long as the slots probing encountered are not k, keep probing until encounter k and replace the slot with "delete me”.If encounter empty slot then fails to delete.

##### Probing strategies
- linear probing: h(k,i) = ( h`(k) + i ) mod m; h`(k) is an ordinary hash function
problems: not random and cluster. once there is a cluster you have to do more operations that works for nothing

- double hashing : h(k,i ) = ( h1(k) + i* h2(k) ) mod m;
If h2(k) is relatively prime to m then we are certain that h(k,i) will have a permutation

#### lecture 11. Integer arithmetic , Karatsuba Multiplication
- Irrationals
- Newton`s Method
- High Precision Multiplication

##### Irrationals
- Catalan numbers
Set P of balanced parentheses strings

- λ ∈ P (λ is the empty string)
- If α ,β ∈ P , then (α)β ∈P

##### Newton`s Method         

－ Root of f(x) = 0 through successive approximation
` xi+1 = xi  - f(xi) / f ` (xi) ;`

- High precision  multiplication
2 n-digits numbers  ( radix r = 2 , 10)
```
0<= x, y <= r^n ;

x = x1 * r^(n/2) + x0;     //x1:high half , x0: low half
y = y1 * r^(n/2) + y0;

0 <= x0,x1 <  r^(n/2);
0 <= y0,y1 <  r^(n/2);

let
z0 = x0 * y0;
z1 = ( x0*y1 + x1*y0 );
z2 = x2 * y2 ;

z = x*y = x1*y1*r^n + (x0*y1 + x1*y0) * r^(n/2) + x0*y0
```
- karatsuba algorithm
for improving the multiplication process
z0 = x0 * y0;
z2 = x2 * y2;
z1 = (x0 + x1 )* ( y0 + y1) - z0 - z1;

#### recitation 11 Principal of Algorithm Design

- Given a shifted sorted array, find the K element where the biggest mets the smallest number in binary search tree.
example:
     7, 8 , 9, 1 , 2 , 3 , 4 , 5 , 6

- the first element, here is 7,is the most important find the first element in binary search.
- if middle is bigger ,search recursively right,otherwise on the left.(if big find bigger , if small find smaller) to find the point where two subarray joined.

- Find Kth smallest element in a minimum heap.

1. because of minimum heap, we can use heap property. Because the root is the smallest element, we use:

`A.pop(root);`

Kth times to get the element we want in O(K * logN) time because retain heap property requires logN time.

2. First chop heap from root to height K,because all paths of length K end to K level and beyond K the element will always be bigger than K because its a min heap.Then find Kth smallest in the heap recursively. Second, set up a augmented heap called horizon which is the container to do the iteration, each time extract-min from it , fill in the min`s children to replace min for that these are the element to find the next min.

pseudo code:
```python
H <- minheap ;
Z <- minheap ; //augmented with index of each element to store the min k element
Z.insert( H[1] , 1 ) ;  //
for i in range(1…k)
     (value,index) = Z.extract-min // get out the min element and replace with its children to compare
     Z.insert( H[2j] , 2j );
     Z.insert( H[2j + 1] , 2j + 1 );
return index;
```
this gives you O(K * logK) running times.

#### lecture 12 Square Root, Newton`s Method
- Multiplication , Division
- Algorithm Complexity

##### High-precision Division
- set f(x) = 1/x - b/R ;
so when we trying to do f(x) = 0, we will get 1/x = b/R  so x = R/b;
applying Newton iteration:
Xi+1 = Xi - f(Xi)/f`(Xi) = 2Xi - (b*Xi^2)/R

#### Recitation
##### Karatsuba Multiplication
usually multiplication take a time of O(N^2) complexity whereas KM takes O( N * log2 3).
It takes 2 N numbers, split it into N/2 numbers and do 3 multiplications and addition then we have T(n) = 3T(N/2) + O(N).

#### lecture 13 Breadth First Search (BFS)
##### Graph Search
graph G = ( V , E ); V is a set of vertices and E is a set of edges.
if the edges have directions then the G is directed graph otherwise is undirected graph.

Applications:
- web crawling
- social networking
- network broadcast
- garbage collection
- model checking
- solve puzzles and games

Graph representation:
- adjacent list:
array Adj of size |V|;
each element in the array is a pointer to link list;
the array is indexed by the vertex so for every vertex v,
Adj[v] = {one step accessible neighbours of v };

##### Breadth first search
- visit all node s reachable from given set V
- O (V + E ) time
- look at nodes reachable in 1 moves , 2 moves …...
- carefully avoid duplicates , not revisiting vertices.
```python
BFS(s,Adj):
level = {s:none};// dictionary with key s and value none;
parent = {s: none}
i = 1;
frontier = [s];
while  frontier:
     next = [];
          for  u in frontier:
               for v in Adj[u]:
                    if v not in level:
                         leve[v] = i;
                         parent[v] = u;
                         next.append(v);
frontier = next;
i += 1;
```

#### lecture 14 Depth First Search (DFS)
- recursively explore the graph,back tracking as necessary
- careful not to repeat vertices

```python
parent = {s : none};#dictionary to prevent repeat
def DFS - Visit( V , Adj , s ): #can visit vertices reachable from s
          for v in Adj[s]:
               if v not in parent:
                    parent[v] = s;
                    DFS-Visit(V,Adj,v);
```
```python
def DFS( V , Adj ): # top level algorithm
parent = {}
for s in V:
     if s not in parent:
          parent[s] = None
          DFS-Visit( V , Adj , s)
```
Analysis: O (V + E );linear time

##### Edge Classification:
- tree edge: (parent pointer) visit new vertex via edge
- forward edges: node -> descendent
- backward edges: node -> ancestor
- crossward edges: all other edges beside the two above, between sibling vertices

##### Cycle Detection:
G has a cycle <=> G has a back edge

##### Topological Sort:
- Job scheduling:
given a undirected acyclic graph (DAG),order vertices so that all edges point from lower order to higher order.

- sort:
run DFS , output the reverse of finish times of vertices

#### lecture 15 Single-Source Shortest Paths Problem
- Dijkstra: non-negative weight edges
complexity: O( v*(lg v) + E);

- Bellman-Ford :+/- weight edges
complexity: O ( v*E ) ;
negative circle can make path weight to infinite

##### weighted graphs
shortest path from u to v is :
delta(u,v) = min { path(u -> v) } if path exist, else  infinite;
d(u) is the current weight represented in side vertices;
Pi[v] is the predecessor vertex on the best path to v; Pi[source] = nil

##### general structure of shortest path algorithm // none negative value
initialise for all u ∈ V  , set d(V) = infinite , set Pi[V] = nil
d(source) = 0;
Repeat select edge(u,v): // somehow
     Relax edge(u,v):
          if d(v) > d(u) + w(u,v):
               d(v) = d(u) + w(u,v);
               Pi(v) = u;
 until all edges have d(v) <= d(u) + w(u,v);

##### optimum substructure property

-  Subpaths of a shortest path are shortest paths

#### lecture 16    Dijkstra`s Algorithm
Relaxation:
d[v] : length of current vertex from source
w(s,v):length of a shortest path from s to v
π[v] : predecessor of v in the path from s to v
```python
Relax( u, v, w  )
    if d[v] > d[u] + w(u,v)
        d[v]  = d[u] + w(u,v)
        π[v] = u
```

Relaxation is safe

Dijkstra:
```python
Dijkstra(G,w,s) // G graph, w weight, s source
    Initialise(G,s)  ;      S = NULL;    Q = V[G];   // S is empty set, Q is the set of all vertexes //initialise set d[s] = 0
    while Q != NULL
        u = Extract_Min(Q);        //delete u from q
        S = S + u;
        for each vertex v belonging to Adjacent(u):
            Relax(u,v,w);
``` 
