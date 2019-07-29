---
layout: post
title:  "원형 이중 연결 리스트(CircularDoublyLinkedList)"
date:   2019-07-15 22:27:00
author: Seungbeom Kim
categories: datastructure_algorithm
tags:	자료구조 알고리즘 원형이중연결리스트
sitemap :
  changefreq : daily
  priority : 1.0
---

### 원형 이중 연결 리스트

원형 단일 연결 리스트는 맨 앞에 값을 넣을 때, 항상 마지막 노드를 찾아야 한다. 하지만 원형 이중 연결 리스트는 입력, 삭제, 찾기 등이 수월하다.

```Java
public class CircularDoublyLinkedList {
	static class Node {
		int data;
		Node next, prev;

		Node(int d) {
			this.data = d;
			this.next = null;
			this.prev = null;
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
			temp.prev = head.prev;
			head.prev.next = temp;
			head = temp;
		}
		else {
			if(target.next == head) {
				head.prev = target.prev;
				target.prev.next = head;
			}
			else {
				Node temp = target.next.next;
				temp.prev = target;
				target.next = temp;
			}

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
			newNode.prev = temp.prev;
			temp.prev.next = newNode;
			temp.prev = newNode;
			head = newNode;
		}
		else {
			if(target.next == head) {
				newNode.next = target.next;
				newNode.prev = target;
				target.next = newNode;
				head.prev = newNode;

			}
			else {
				temp = target.next;
				temp.prev = newNode;
				newNode.next = temp;
				newNode.prev = target;
				target.next = newNode;
			}
		}
	}

	public static void addNode(int data) {
		if(head == null) {
			head = new Node(data);
			head.prev = head;
			head.next = head;
		}
		else {
			Node target = head;
			while(target.next != head) {
				target = target.next;
			}

			target.next = new Node(data);
			target.next.next = head;
			target.next.prev = target;
			head.prev = target.next;
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

		Node prev = head;
		Node temp = head;
		while(temp.next != head) {
			System.out.format("%d ", temp.data);
			temp = temp.next;

			if(temp.next == head) {
				System.out.format("%d ", temp.data);
			}
		}

		System.out.println("\n");

		while(prev.prev != head) {
			System.out.format("%d ", prev.data);
			prev = prev.prev;

			if(prev.prev == head) {
				System.out.format("%d ", prev.data);
			}
		}
	}
}
```
