---
layout: post
author: study
title:  "자바스크립트 : 이진탐색트리(BST)"
description: "연습 문제"
categories: [ Study ]
tags: [programming, javascript]
---


# BST를 만들어 보자.

 우선 Data를 담을 노드.


  ```javascript
  class Node{
    constructor(data, left, right){
      this.data = data;
      this.left = left;
      this.right = right;
    }
    show(){
      return this.data;
    }
  }
  ```

 BST, Right Node가 존재해야 Successor를 찾는 것으로 들어가므로 굳이 Successor를 사용해서 중복 제한을 하는 것 보다 Minimum 값을 사용하는게 좋아 보인다.

  ```javascript
  class BST{
    constructor(){
      this.root = null;
    }
  
    getRoot(){
      return this.root;
    }
  
    insert(data){
      let n = new Node(data, null, null);

      if(this.root == null){
        this.root = n;
      }
      else{
        let current = this.root;
        let parent;
        while(true){
          parent = current;
          if(data < current.data){
            current = current.left;
            if(current == null){
              parent.left = n;
              break;
            }
          } else {
            current = current.right;
            if(current == null){
              parent.right = n;
              break;
            }
          }
        }
      }
    }
  
    inOrder(node){
      if(!(node == null)) {
        this.inOrder(node.left);
        console.log(node.show());
        this.inOrder(node.right);
      }
    }
  
    find(data){
      let current = this.root;
      while(current.data != data)
      {
        if(data < current.data) {
          current = current.left;
        } else {
          current = current.right;
        }

        if(current == null){
          return null;
        }
      }
      return current;
    }

    remove(data){
      this.root = this.removeNode(this.root, data);
    }

    removeNode(node, data){
      if(node == null){
        return null;
      }
      if(data == node.data){
        if(node.left == null && node.right == null){
          return null;
        }

        if(node.left == null){
          return node.right;
        }

        if(node.right == null){
          return node.left;
        }

        let tempNode = this.getMinimum(node.right);
        node.data = tempNode.data;
        node.right = this.removeNode(node.right, tempNode.data);
        return node;
      } else if(data < node.data) {
        node.left = this.removeNode(node.left, data);
        return node;
      } else {
        node.right = this.removeNode(node.right, data);
        return node;
      }
    }

    getMinimum(node){
      let current = node;
      while(!(current.left == null))
      {
        current = current.left;
      }
      return current;
    }
  } 
  ```

 이제 문제 나오면 사용하도록 하자.
