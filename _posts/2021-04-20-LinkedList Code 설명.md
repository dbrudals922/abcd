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
```
public class Node {
	public int data; // 변수 생성
	public Node next; // 변수 생성

	public Node(int data) {
		this.data = data; // 값 설정
	}

	public void print() {
		System.out.print("{" + data + "}");  // 값 출력
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
