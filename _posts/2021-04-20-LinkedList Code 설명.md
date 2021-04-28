---
layout: post
title: "LinkedList 코드 설명"
tags: [java]
comments: true
---

뭔지 모르겠는 LinkedList 관련된 코드들 설명하는 글임.
어떻게 설명해야될지 모르겠음. <br>다 연관되어있어서 따로 설명하기도 그렇고 따로 끊어야할지 그냥 주석으로 쓴 코드를 통으로 올릴지 모르겠음.

---

## 1. Node 코드
```java
public class Node {
	public int data;
	public Node next;

	public Node(int data) { // 생성자
		this.data = data; // 
	}

	public void print() {
		System.out.print("{" + data + "}");  // 값 출력
	}
}
```

## 2. LinkedListMain 코드
```java
public class LinkedListMain {
	public static void main(String[] args) {
		LinkedList linkedList = new LinkedList(); // 객체 생성
		linkedList.addLast(1); // 리스트 맨 마지막에 값 추가
		linkedList.addLast(2);
		linkedList.addLast(3);
		linkedList.addLast(4);
		linkedList.addLast(5);
		linkedList.add(2, 8); // 위치, 값 순서로 원하는 위치에 값을 추가
		linkedList.delete(4); // 그 위치에 값을 삭제
		linkedList.addFirst(0); // 값을 맨 앞에 추가 
		while(!linkedList.isEmpty()) { // 리스트가 비어있지 않다면
			Node delLink = linkedList.deleteFirst(); // 맨 앞에 있는 요소 삭제
			delLink.print(); // 리스트 프린트
			System.out.println(); // 빈줄
		}
	}
}
```

## 7. Code Snippets

### Highlighted Code Blocks

```css
#container {
  float: left;
  margin: 0 -240px 0 0;
  width: 100%;
}
```

### Standard code block

    <div id="awesome">
      <p>This is great isn't it?</p>
    </div>
