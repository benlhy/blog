---
title: "Building a D3 Binary Tree"
date: 2020-04-19T08:48:40+00:00
slug: "building-an-animated-d3-binary-tree"
draft: false
cover:
  image: "/images/2020/04/binarytree-3.JPG"
  relative: false
  hiddenInList: false
---

Tldr: I built an animated binary tree following the lessons learned from the D3 course on Frontend Masters by Shirley Wu as well as Brian Holt's Introduction to Computer Science.

Play with the tree here: [https://bl.ocks.org/benlhy/83a93740449b29ab731184bd233c9994/8f68c0d8ed273ac075102a5e7af886559d80eb81](https://bl.ocks.org/benlhy/83a93740449b29ab731184bd233c9994/8f68c0d8ed273ac075102a5e7af886559d80eb81)

{{< figure src="/images/2020/04/image-3.png" alt="" caption="Randomly generated tree" >}}

A binary tree is a type of data structure that sorts new values. Each node in a binary tree has two children, the left is always smaller than the parent node, while the right is always larger than the parent node. By following this rule, a binary tree is constructed.

Binary trees are used for sorting and searching data. They can be very efficient, as the number of steps required to find the data is the height of the tree, and the height has a $\log{n}$ relationship with the data. This makes them very efficient relative to other forms of representing data.

I wanted to build this to check my understanding as well as to build on the things that I learned during the courses. During the course, there were exercises on CodePen, and I liked the visualization, although I thought it was a little simple and could be improved with some animation that I learned with D3.

D3 has a native tree display, but it is catered towards the display of collapsable hierarchical data, and there was no way to define left and right nodes, which is important in a binary tree. Therefore I decided to create my own tree display.

Nodes are randomly generated from 1 ~ 100 and are added to the tree sequentially. I found that instead of displaying the whole tree at once, adding each node and seeing how the tree evolved helped in my understanding of how binary trees work. The colored nodes indicate the path which the new node was added, where the blue nodes are nodes which were tested to be smaller than the newest node, and red when they were tested to be larger than the newest node.

D3 is used to visualize the tree, as well as to animate new links growing from their parent node and the creation of new nodes.
