---
layout: post
author: study
title:  "Graph - DFS/BFS"
description: "그래프 그리고 DFS/BFS"
categories: [ study ]
tags: [programming, kotlin]
postImgOn: false
image: assets/images/study/kotlin.png
---

### BFS와 DFS

[그래프](../study_Programming_Graph)를 가지고 BFS와 DFS를 통해 전체 노드를 순환해보자.

편의상 그래프 중 무방향 그래프를 대상으로 하고자 한다.

<div class="card h-100 my-u-padding"><div class="insertcover"><div class=""><img class="inserturl" src="{{ site.baseurl }}/assets/images/study/Graph.png" alt="programmers.co.kr"/></div></div></div>

| Vertex |	Edge | 
| :---: | :---: | 
| 8 | [[1, 2], [1, 3], [2, 4], [2, 5], [3, 6], [3, 7], [4, 8], [5, 8], [6, 8], [7, 8]] |


배열과 리스트 둘다 가능하다. 그래서 두개의 사례 모두 작성해 놓겠지만, 아마도 리스트를 주로 쓸것 같다.

### DFS
- 노드를 설정한 방향대로 끝까지 간 뒤 가장 최근으로 backtacking을 진행하여 해당 방향 끝까지 보내며 전체 탐색을 하는 방법
- 재귀 호출 방법과 스택을 사용한 방법이 있다.

<center>1→2→4→8→5→(8)→6→3→(6)→(8)→7→(8)→(4)→(2)→(1)</center>

##### 배열
- 재귀

```java
fun dfsSearch(arr: Array<Array<Int>>, start: Int) {
    val visited = Array(arr.size) { false }
    fun recDfs(index: Int) {
        visited[index] = true
        print("$index ")

        arr[index].forEachIndexed {index, item ->
            if(item == 1) {
                if(!visited[index]) recDfs(index)
            }
        }
        return
    }
    return recDfs(start)
}
dfsSearch(arr, 1)
```
```
1 2 4 8 5 6 3 7 
```
- 스택

```java
fun dfsSearch(arr: Array<Array<Int>>, start: Int) {
    val visited = Array(arr.size) { false }
    val stack = Stack<Int>()
    fun stackDfs(index: Int) {
        stack.push(index)
        var flag: Boolean
        visited[index] = true
        print("$index ")
        var value:Int

        while(stack.isNotEmpty()) {
            value = stack.peek()
            flag = false
            run loop@ {
                arr[value].forEachIndexed{ index, item ->
                    if(item == 1) {
                        if(!visited[index]) {
                            stack.push(index)
                            print("$index ")
                            visited[index] = true
                            flag = true
                            return@loop
                        }
                    }
                }
            }
            if(!flag) stack.pop()
        }
        return
    }
    return stackDfs(start)
}
dfsSearch(arr, 1)
```
```
1 2 4 8 5 6 3 7 
```

##### 리스트
- 재귀

```java
fun dfsSearch(list: List<ArrayList<Int>>, start: Int) {
    val visited = Array(list.size) { false }
    fun recDfs(index: Int) {
        visited[index] = true
        print("$index ")

        list[index].forEach{
            if(!visited[it]) recDfs(it)
        }
        return
    }
    return recDfs(start)
}
dfsSearch(list, 1)
```
```
1 2 4 8 5 6 3 7 
```

- 스택

```java
fun dfsSearch(list: List<ArrayList<Int>>, start: Int) {
    val visited = Array(list.size) { false }
    val stack = Stack<Int>()
    fun stackDfs(index: Int) {
        stack.push(index)
        var flag: Boolean
        visited[index] = true
        print("$index ")
        var value: Int

        while(stack.isNotEmpty()) {
            value = stack.peek()
            flag = false
            run loop@{
                list[value].forEach {
                    if (!visited[it]) {
                        stack.push(it)
                        print("$it ")
                        visited[it] = true
                        flag = true
                        return@loop
                    }
                }
            }
            if(!flag) stack.pop()
        }
        return
    }
    return stackDfs(start)
}
dfsSearch(list, 1)
```
```
1 2 4 8 5 6 3 7 
```

## BFS

- 한 노드에 연결된 노드를 전체 탐색한 뒤 설정한 방향대로 차례로 진행하며 전체를 탐색 하는 방법
- 큐를 사용한다.

<center>1→(1)→2→3→(2)→4→5→(3)→6→7→(4)→8→(5)→(6)→(7)→(8)</center>

##### 배열

```java
fun bfsSearch(arr: Array<Array<Int>>, start: Int) {
    val visited = Array(arr.size) { false }
    val queue: Queue<Int> = LinkedList<Int>()

    fun bfs(index: Int) {
        queue.add(index)
        var flag: Boolean
        visited[index] = true
        print("$index ")
        var value: Int

        while(queue.isNotEmpty()) {
            value = queue.peek()
            flag = false
            run loop@{
                arr[value].forEachIndexed { index, item ->
                    if(item == 1) {
                        if (!visited[index]) {
                            queue.add(index)
                            print("$index ")
                            visited[index] = true
                            flag = true
                            return@loop
                        }
                    }
                }
            }
            if(!flag) queue.poll()
        }
        return
    }
    return bfs(start)
}
bfsSearch(arr, 1)
```
```
1 2 3 4 5 6 7 8 
```

##### 리스트

```java
fun bfsSearch(list: List<ArrayList<Int>>, start: Int) {
    val visited = Array(list.size) { false }
    val queue: Queue<Int> = LinkedList<Int>()

    fun bfs(index: Int) {
        queue.add(index)
        var flag: Boolean
        visited[index] = true
        print("$index ")
        var value: Int

        while(queue.isNotEmpty()) {
            value = queue.peek()
            flag = false
            run loop@{
                list[value].forEach {
                    if (!visited[it]) {
                        queue.add(it)
                        print("$it ")
                        visited[it] = true
                        flag = true
                        return@loop
                    }
                }
            }
            if(!flag) queue.poll()
        }
        return
    }
    return bfs(start)
}
bfsSearch(list, 1)
```
```
1 2 3 4 5 6 7 8 
```
