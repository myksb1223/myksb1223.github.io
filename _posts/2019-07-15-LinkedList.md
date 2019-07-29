---
layout: post
title:  "연결 리스트(LinkedList)"
date:   2019-07-15 22:27:00
author: Seungbeom Kim
categories: datastructure_algorithm
tags:	자료구조 알고리즘 연결리스트
sitemap :
  changefreq : daily
  priority : 1.0
---

### 연결 리스트

배열은 크기가 정해진 상태이기 때문에 입력에 제약이 있고 삭제시 메모리 낭비가 발생한다. 연결 리스트는 이러한 부분을 해결해 주는 자료구조이다.

```Java
public class LinkedList {

	static class Node {
		int data;
		Node next;

		Node(int d) {
			this.data = d;
			this.next = null;
		}
	}

	private static Node head = null;

	public static void deleteNode(int index) {
		Node target = null;

		for(int i=0; i<index; i++) {
			if(target == null) {
				target = head;
			}
			else {
				target = target.next;
			}
		}

		if(target == null) {
			Node temp = head.next;
			head = temp;
		}
		else {
			Node temp = target.next.next;
			target.next = temp;
		}
	}

	public static void addNode(int data, int index) {
		Node target = null;
		for(int i=0; i<index; i++) {
			if(target == null) {
				target = head;
			}
			else {
				target = target.next;
			}
		}

		Node temp = null;
		Node newNode = new Node(data);
		if(target == null) {
			temp = head;
			newNode.next = temp;
			head = newNode;
		}
		else {
			temp = target.next;
			newNode.next = temp;
			target.next = newNode;

		}
	}

	public static void addNode(int data) {
		if(head == null) {
			head = new Node(data);
		}
		else {
			Node target = head;
			while(target.next != null) {
				target = target.next;
			}

			target.next = new Node(data);
		}

	}

	public static void main(String[] args) throws IOException {

		addNode(1);
		addNode(2);
		addNode(3);
		addNode(4);
		addNode(5);
		addNode(6);
		addNode(7);

		addNode(9, 2);
		addNode(9, 1);
		addNode(10, 0);

		deleteNode(0);
		deleteNode(1);
		deleteNode(2);

		Node temp = head;
		while(temp != null) {
			System.out.format("%d ", temp.data);
			temp = temp.next;
		}
	}
}
```
