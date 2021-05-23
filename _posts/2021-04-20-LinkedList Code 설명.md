---
layout: post
title: "LinkedList 코드 설명"
description: "LinkedList 코드를 알아보자."
date: 2021-04-20
tags: [java]
comments: true
share: true
---

본격 LinkedList 코드 설명하는 글.

---


## LinkedList 코드★

```java
import java.util.ArrayList;
import java.util.Iterator;

public class LinkedList {
	private Node head;

```
여기까진 그냥 기본<br>
맨 처음 head 변수 만들어줌 <br>

``` java
	public LinkedList(){
		head = null;
	}
``` 
맨 처음에는 리스트 비워줌
<br>
<br>
<br>
값을 맨 앞에 넣어주는 메소드
``` java
	public void addFirst(int value){
		Node link = new Node(value);
		link.next = head;
		head = link;
	}
```
<br>
새 노드를 생성한 뒤 새로 추가하는 노드의 next를 원래 first 노드로 해줌 <br>
그리고 새로 추가하는 노드를 first로 지정해줌<br>
<br>
<br>
<br>
값을 맨 뒤에 넣어주는 메소드
<br>
``` java
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
		else lastLink.next = link;
	}
```
<br>
link가 새로 추가하는 노드라는 건 동일<br>
tmpLink를 first 노드로, lastLink는 null로<br>
만약 리스트에 값이 있었다면<br>
lastLink를 tmpLink로, tmpLink를 그 다음 노드로 지정해주며 한단계씩 앞으로 전진함.<br>
**그러면 결국 tmpLink는 맨 마지막 null 까지 가게되고, lastLink는 null 바로 앞 노드의 값이 저장되어있음**<br>
그래서 lastLink의 next를 새로 추가하는 노드로 지정<br>
만약 리스트에 값이 없었다면 그냥 새로 추가하는 노드를 first로 지정
<br>
<br>
<br>
원하는 위치의 값을 가져오는 메소드
```java
	public Node get(int index) {
	    Node node = head;
	    for (int i = 0; i < index; i++)
	        node = node.next;
	    return node; // 리턴
	}
```
<br>
맨 처음 노드부터 index 번째 노드까지 for문으로 값을 가져옴
<br>
<br>
<br>
원하는 위치에 값을 저장하는 메소드
```java
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
```
<br>
추가할 인덱스 앞 노드의 next를 새로 추가한 노드로, 새로운 노드의 next를 이전노드의 next로 바꾸는 작업 필요<br>
newNode 변수에 새로운 노드를 저장한후 previosNode의 next값으로 지정, newNode의 next를 preiviosNode의 원래 next로 지정<br>
간단하쥬? *tktlf gkskeh dksrkseksgka.. wnrdmfrjrkxdma..*

```java
	public void addFirst(int value){
		Node link = new Node(value);
		link.next = head;		//	새로 추가하는 노드의 next를 앞 노드로 지정
		head = link;				//	새로 추가하는 노드를 first로 지정
	}
```
맨 앞에 노드를 추가하는 코드<br>
새로운 노드의 next를 앞 노드로 지정해주고 first로 지정해주면 끄읕
<br>
<br>
```java
	public Node deleteFirst(){
		Node link = head;	// 제일 앞의 first부터 값을 리턴
		if(head == null){	// 만약 비어있다면
			return null;	// 빈 값을 보낸다
		}
		head = head.next;
		return link;
	}

```
맨 앞 노드 삭제<br>
head의 다음 노드를 head에 연결
<br>
<br>
<br>
```java
	public Object delete(int index){
	    if(index == 0)
	        return deleteFirst();  // 
	    Node previosNode = get(index-1);				//	삭제할 인덱스 앞 요소(이전노드)
	    Node deleteNode = previosNode.next;				//	이전노드의 링크 노드는 삭제할 노드, 지금 삭제하면 노드를 연결할 수 없다. 
	    previosNode.next = deleteNode.next;				//	삭제할 노드의 링크노드가 이전노드의 링크노드가 되어야 삭제할 노드와의 연결이 끊어진다.
	    Object returnValue = deleteNode.data; 			//	삭제할 노드의 값을 리턴하기 위해 저장
	    return returnValue;
	}
```
특정 위치 노드 삭제하는 코드<br>
<br>
<br>
```java
	public Node get(int index) {
	    Node node = head;
	    for (int i = 0; i < index; i++) // 0부터 인덱스까지 돌림
	        node = node.next; 
	    return node;
	}
```
인덱스 전 노드를 찾아서 next 리턴<br>
<br>
<br>
나머지 등등↓
```java
	public boolean isEmpty(){
		return head == null;
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
<br>
그래서 전체코드는
```java
import java.util.ArrayList;
import java.util.Iterator;

public class LinkedList {
	private Node head;

	public LinkedList(){
		head = null; 			// 처음 생성할때 값을 비워줌
	}
	
	public boolean isEmpty(){	// 리스트가 비어있는지 확인해줌
		return head == null;
	}
	
	// node에 계속 다음 노드를 index 까지 넣어주다 멈춤
	public Node get(int index) {
	    Node node = head; // node 를 맨 처음 노드 주소로 함
	    for (int i = 0; i < index; i++) // index번째 까지 돌아감
	        node = node.next;	// for문이 도는동안 계속 다음 노드로 바꿈
	    return node; // 리턴
	}

	public void addFirst(int value){
		Node link = new Node(value); // 새 노드 생성
		link.next = head;		//	새로 추가하는 노드의 next를 앞 노드로 지정
		head = link;			//	새로 추가하는 노드를 first로 지정
	}
	public void addLast(int value){
		Node link = new Node(value); // 새로운 노드 생성

		//	마지막까지 보내는 구문
		Node tmpLink = head;	// tmpLink에 맨 앞 노드 저장
		Node lastLink = null;
		while(tmpLink != null) {	// list에 값이 있다면
			lastLink = tmpLink;		// lastLink에 현재 노드를 저장
			tmpLink = tmpLink.next; // tmpLink가 다음 노드 저장
		}
		if(lastLink == null) head = link;	// list에 값이 없다면 맨 앞에 값 추가됨
		else lastLink.next = link;				//	새로 추가하는 노드를 first로 지정
	}
	
	/*
	 * 가장 마지막 값을 얻기 위해 lastLink 변수 사용
	 * lastLink의 다음 노드를 연결해줌
	 */
	
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
		head = head.next;	// head는 다음 값을 지정
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
