---
title: LintCode Medium Java Solution
date: 2016-08-27 19:15:50
tags:
---

##### Ugly Number II
- Ugly number is a number that only have factors 2,3 and 5.Design an algorithm to find the nth ugly number. The first 10 ugly numbers are1, 2, 3, 4, 5, 6, 8, 9, 10, 12...

###### solution:
- triple pointer: because the Ugly number array is an array that starts from 1 and by multiply 2,3,5 repeatedly to the numbers in the array.So the idea is to point at numbers in the array and check if the number multiplied by 2,3,5 is greater then the last number.
- 三个指针指向一个数列，如果指向目标乘2、3、5大于数列最后一个数，则从三者中选最小的加入数列

```java
public int nthUglyNumber(int n) {
               List<Integer> uglys = new ArrayList<Integer>();
        uglys.add(1);

        int p2 = 0, p3 = 0, p5 = 0;
        // p2, p3 & p5 share the same queue: uglys

        for (int i = 1; i < n; i++) {
            int lastNumber = uglys.get(i - 1);
            while (uglys.get(p2) * 2 <= lastNumber) p2++;
            while (uglys.get(p3) * 3 <= lastNumber) p3++;
            while (uglys.get(p5) * 5 <= lastNumber) p5++;

            uglys.add(Math.min(
                Math.min(uglys.get(p2) * 2, uglys.get(p3) * 3),
                uglys.get(p5) * 5
            ));
        }

        return uglys.get(n - 1);
    }
```

<!--more-->
##### Binary Tree Serialization
- Design an algorithm and write code to serialize and deserialize a binary tree. Writing the tree to a file is called 'serialization' and reading back from the file to reconstruct the exact same binary tree is 'deserialization'.
- There is no limit of how you deserialize or serialize a binary tree, you only need to make sure you can serialize a binary tree to a string and deserialize this string to the original structure.

##### solution:
maintain a list of TreeNode to keep track of nodes postions 
```
     public String serialize(TreeNode root) {
        if (root == null) {
            return "{}";
        }

        ArrayList<TreeNode> queue = new ArrayList<TreeNode>();
        queue.add(root);

        for (int i = 0; i < queue.size(); i++) {
            TreeNode node = queue.get(i);
            if (node == null) {
                continue;
            }
            queue.add(node.left);
            queue.add(node.right);
        }

        while (queue.get(queue.size() - 1) == null) {
            queue.remove(queue.size() - 1);
        }

        StringBuilder sb = new StringBuilder();
        sb.append("{");
        sb.append(queue.get(0).val);
        for (int i = 1; i < queue.size(); i++) {
            if (queue.get(i) == null) {
                sb.append(",#");
            } else {
                sb.append(",");
                sb.append(queue.get(i).val);
            }
        }
        sb.append("}");
        return sb.toString();
    }
```

```
    public TreeNode deserialize(String data) {
        if (data.equals("{}")) {
            return null;
        }
        String[] vals = data.substring(1, data.length() - 1).split(",");
        ArrayList<TreeNode> queue = new ArrayList<TreeNode>();
        TreeNode root = new TreeNode(Integer.parseInt(vals[0]));
        queue.add(root);
        int index = 0;
        boolean isLeftChild = true;
        for (int i = 1; i < vals.length; i++) {
            if (!vals[i].equals("#")) {
                TreeNode node = new TreeNode(Integer.parseInt(vals[i]));
                if (isLeftChild) {
                    queue.get(index).left = node;
                } else {
                    queue.get(index).right = node;
                }
                queue.add(node);
            }
            if (!isLeftChild) {
                index++;
            }
            isLeftChild = !isLeftChild;
        }
        return root;
    }
```