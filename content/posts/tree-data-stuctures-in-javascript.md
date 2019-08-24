---
title: "Tree Data Structure in Javascript"
date: "2019-08-24T11:46:37.121Z"
template: "post"
draft: false
slug: "/posts/tree-data-structure-in-javascript/"
category: "Javascript"
tags:
  - "Programming"
  - "Javscript"
  - "Tree"
  - "Binary Search Tree"
description: "In this I'll be explaining about Tree data structure. The main topics will be what is tree and binary search tree and how we can implement binary search tree using Javascript."
---

![Trees](/media/post6-image1.png)


##What is a Tree?
Trees are well-known as non-linear data structures. This means they don't store their data in a linear way like (array, stack, queue, etc. stores), they store their data in a hierarchically way.
A tree is a data structure where a node can have zero or more children. Each node contains a value or data and it may or may not have a ***child node***. The connection between the nodes is called ***edges***. The first node of the tree is called the ***root***. If the ***root*** is connected by another node then ***root*** is called as ***parent*** node and connected node is ***child*** node. The nodes that don't have any children are *** leaves *** nodes.

###Trees main properties:
- **Root** is the topmost `node` of the tree.
- **Edge** is the link between two `nodes`.
- **Child** is `node` that has a parent node.
- **Parent** is `node` that has an edge to the `child node`.
- **Leaf** is `node` that doesn't have a `child node` in the `tree`. 
- **Height** is the length of the longest path to the `leaf`.
- **Depth** is the length of the path to its `root`.

##Implementing a simple tree data structure
As we saw earlier, a tree node just a data structure that has a value and has links to their descendants.

Here is an example of the Document Object Model (DOM) tree:
```js
// Creating a TreeNode
class TreeNode {
  constructor(value) {
    this.value = value
    this.descendents = []
  }
}
// Create nodes with value
var html = new TreeNode('HTML')
var head = new TreeNode('HEAD')
var body = new TreeNode('BODY')
var meta = new TreeNode('META')
var title = new TreeNode('TITLE')
var ul = new TreeNode('UL')
var h1 = new TreeNode('H1')
var h3 = new TreeNode('H3')

html.descendents.push(head, body)
body.descendents.push(ul, h1, h3)
head.descendents.push(meta, title)

console.log(html)

// Visually, it looks like this

/*
                          HTML
                      /          \
                    HEAD           BODY
                  /     \        /  |   \
                META    TITLE  UL   H1   H3
                
*/
```

##Difference between Binary Tree and Binary Search Tree?

In a ***binary tree***, each node can have a maximum of 2 child nodes, and it doesn't follow any order in terms of how the nodes should be organized in the tree. 

***Binary search tree*** is essentially a binary tree, in terms of how much child nodes a node in the binary search tree can possibly have, but there is one important difference that is: In binary search tree there is relative ordering in how the nodes are organized. In BST, the left node is smaller than its parent node and the right node is greater than its parent node.

##Let's implement a Binary search tree
BST is very similar to our previous implementation of a simple tree. However, there are some differences:
- Nodes can have most only two children, right and left.
- Node values has to be ordered `left < parent < right`.

Let's start:
```js
// Tree Node
class TreeNode {
  constructor(data, left=null, right=null) {
    this.data = data
    this.left = left
    this.right = right
  }
}

class BST {
  constructor() {
    this.root = null
  }
  add(data) {
    /*...*/
  }
  findMin() {
    /*...*/
  }
  findMax() {
    /*...*/
  }
  find(data) {
    /*...*/
  }
  isPresent(data) {
    /*...*/
  }
  remove(data) {
    /*...*/
  }
}
```
Let us implement insertion of tree node:
### BST (Binary Search Tree) node insertion
To insert a binary node, we do the following:
- if a tree is empty, the first root becomes the root node and you are done.
- Now you compare the `new value` with the `root node value` if its higher go `right`, it's lower go `left`. If it's the same, don't do anything become it already exists.
- Repeat the above step until you find an empty location to insert the new value. 

Let us write the code for above-defined steps:
```js
add(data) {
  var node = this.root
  if (node === null) {
    this.root = new Node(data)

    return
  } else {
    const searchTree = function(node) {
      if (data < node.data) {
        if (node.left === null) {
          node.left = new Node(data)
        } else if (node.left !== null) {
          return searchTree(node.left)
        }
      } else if (data > node.data) {
        if (node.right === null) {
          node.right = new Node(data)
        } else if (node.right !== null) {
          return searchTree(node.right)
        }
      } else {
        return null
      }
    }

    return searchTree(node)
  }
}
```

###Find Smallest value on the Tree
To find the smallest value, we do the following:
- Binary Search Tree stores the smaller value on the left node of the current node, to find the smallest value, we need to find left least node of the tree.
- Check if the current left `node` is `null`, return the current node data.
- Repeat step 2 until you find the current left `node` value `null`.

Let us write the code for above-defined steps:
```js
findMin() {
  const current = this.root
  while (current.left !== null) {
    current = current.left
  }

  return current.data
}
```

###Find the largest value on the Tree
To find the largest value, we do the following:
- BST stores the larger value on the right of the current node, to find the largest value, we need to find the right least node of the tree.
- Check if current right `node` is `null`, return the current node data.
- Repeat step 2 until you find the current right `node` value `null`.

Let us write the code for above-defined steps:
```js
findMax() {
  const current = this.root
  while (current.right !== null) {
    current = current.right
  }

  return current.data
}
```

###Check if a node is Present on the Tree
To check if the node is present on the tree, we do the following:
- Check if current `node` value is equal to the `new value` then return `true` and you are done.
- Otherwise, compare the `new value` with current `node value` if it is smaller, then repeat the step 1 on left `node` if it is larger, repeat the step 1 on right `node`.
- If you didn't find any matched value, return `false`.

Let us write the code for the above-defined steps:
```js
isPresent(data) {
  const current = this.root
  while (current) {
    if (current.data == data) {
      return true
    }

    if (data > current.data) {
      current = current.right
    } else {
      current = current.left
    }
  }

  return false
}
```

###BST (Binary Search Tree) node deletion
We know how to insert and search a node. Now, we are going to implement the deletion operation. It's little difficult than adding a node, follow these steps to implement it:
- Deletion `node` with no children, then remove it and you are done.
- Deletion `node` with one child, in this case, a node is cut from the tree and the single child directly link to the parent node of the removed `node`.
- Deletion `node` with two children, find minimum value in the right subtree; replace the value of the node to be removed with found minimum value. Now right subtree contains a duplicate! Apply the remover to the right subtree to remove a duplicate.

Let us write the code for above-defined steps:
```js
remove(toBeRemove) {
  const removeNode = function(node, data) {
    if (node == null) {
      return null
    }

    if (node.data == data) {
      if (node.left == null && node.right == null) {
        return
      }
      if (node.left == null) {
        return node.right
      }
      if (node.right == null) {
        return node.left
      }
      const tempNode = node.right

      while(tempNode.left !== null) {
        tempNode = tempNode.left
      }

      node.data = tempNode.data
      node.right = removeNode(node.right, tempNode.data)

      return node
    } else if (node.data > data) {
      node.right = removeNode(node.right, data)

      return node
    } else if (node.data < data) {
      node.left = removeNode(node.left, data)

      return node
    }

    this.root = removeNode(this.root, toBeRemove)
  }
}
```

I hope you must have enjoyed it. I write articles like this every week, you can check those out here [mountainfirefly.dev](https://mountainfirefly.dev).

[I tweet about JavaScript and code tips here.](https://twitter.com/mountainfirefly)
