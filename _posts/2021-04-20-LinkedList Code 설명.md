---
layout: post
title: "LinkedList 코드 설명"
description: "LinkedList와 관련된 코드 3개를 알아보자"
date: 2021-04-20
tags: [java]
comments: true
share: true
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
		this.data = data; // 값 지정
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

## 3. LinkedList 코드★
```java
import java.util.ArrayList;
import java.util.Iterator;


public class LinkedList {
	private Node head;

	public LinkedList(){
		head = null; // 처음 생성할때 값을 비워줌
	}
	public boolean isEmpty(){
		return head == null; // 비어있는지 확인해줌
	}
	public Node get(int index) {
	    Node node = head;
	    for (int i = 0; i < index; i++) // 0부터 인덱스까지 돌림
	        node = node.next; 
	    return node;
	}
	public void addFirst(int value){
		Node link = new Node(value);
		link.next = head;		//	새로 추가하는 노드의 next를 앞 노드로 지정
		head = link;				//	새로 추가하는 노드를 first로 지정
	}
	public void addLast(int value){
		Node link = new Node(value); 

		//	마지막까지 보내는 구문
		Node tmpLink = head;
		Node lastLink = null;
		while(tmpLink != null) {
			lastLink = tmpLink;
			tmpLink = tmpLink.next;
		}
		if(lastLink == null) head = link;
		else lastLink.next = link;				//	새로 추가하는 노드를 first로 지정
	}
	public void add(int index, int value){
		// index가 0이면 첫번째 노드에 추가
		if(index == 0){
			addFirst(value);
		} else {
			Node previosNode = get(index-1);	//	추가할 인덱스 앞 요소(이전노드)
			Node nextNode = previosNode.next;	//	이전노드의 링크 노드는 새로운 노드의 링크가 되어야 함
			Node newNode = new Node(value);
			previosNode.next = newNode;		//	이전노드의 링크 노드는 새로운 노드
			newNode.next = nextNode;		//	새로운 노드의 링크는 이전노드가 가르켰던 노드
		}
	}
	public int size(){
		int count = 0;
		while(!isEmpty()) {
			deleteFirst();
			count++;
		}
		return count;
	}
	public Iterator<Integer> listIterator(){
		ArrayList<Integer> list = new ArrayList<Integer>();
		while(!isEmpty()) {
			Node delLink = deleteFirst();
			list.add(delLink.data);
		}
		return list.iterator();
	}
	public Node deleteFirst(){
		Node link = head;	// 제일 앞의 first부터 값을 리턴
		if(head == null){	// 만약 비어있다면
			return null;	// 빈 값을 보낸다
		}
		head = head.next;
		return link;
	}
	public Object delete(int index){
	    if(index == 0)
	        return deleteFirst();  // 
	    Node previosNode = get(index-1);				//	삭제할 인덱스 앞 요소(이전노드)
	    Node deleteNode = previosNode.next;				//	이전노드의 링크 노드는 삭제할 노드, 지금 삭제하면 노드를 연결할 수 없다. 
	    previosNode.next = deleteNode.next;				//	삭제할 노드의 링크노드가 이전노드의 링크노드가 되어야 삭제할 노드와의 연결이 끊어진다.
	    Object returnValue = deleteNode.data; 			//	삭제할 노드의 값을 리턴하기 위해 저장
	    return returnValue;
	}
	public void print() {
		Node link = head;
		while(link != null) {
			link.print();
			link = link.next;
		}
		System.out.println();
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
