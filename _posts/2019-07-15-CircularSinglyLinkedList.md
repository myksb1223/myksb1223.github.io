---
layout: post
title:  "원형 단일 연결 리스트(CircularSinglyLinkedList)"
date:   2019-07-15 22:47:00
author: Seungbeom Kim
categories: datastructure_algorithm
tags:	자료구조 알고리즘 원형단일연결리스트
sitemap :
  changefreq : daily
  priority : 1.0
---

### 원형 단일 연결 리스트

연결 리스트는 노드 순회를 위해서는 임시적은 포인터가 필요하다. 해더포인터를 건드리면 다시는 이전으로 돌아갈 수 없기 때문이다. 원형 단일 연결 리스트는 마지막 노드를 맨 처음 노드를 가리키게 하는 구조이다. 맨 앞에 값을 넣을 때, 항상 마지막 노드를 찾아야 한다.

```Java
public class CircularSinglyLinkedList {

	static class Node {
		int data;
		Node next;

		Node(int d) {
			this.data = d;
			this.next = null;
		}
	}

	private static Node head = null;
	private static int size = 0;

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
			Node last = head;
			while(last.next != head) {
				last = last.next;
			}

			Node temp = head.next;
			head = temp;
			last.next = head;
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
			Node last = head;
			while(last.next != head) {
				last = last.next;
			}

			temp = head;
			newNode.next = temp;
			head = newNode;
			last.next = newNode;
		}
		else {
			if(target.next == head) {
				target.next = newNode;
				newNode.next = head;
			}
			else {
				temp = target.next;
				newNode.next = temp;
				target.next = newNode;
			}
		}

		size++;
	}

	public static void addNode(int data) {
		if(head == null) {
			head = new Node(data);
			head.next = head;
		}
		else {
			Node target = head;
			while(target.next != head) {
				target = target.next;
			}

			target.next = new Node(data);
			target.next.next = head;
		}

		size++;
	}

	public static void main(String[] args) throws IOException {

		addNode(1);
		addNode(2);
		addNode(3);
		addNode(4);
		addNode(5);
		addNode(6);
		addNode(7);

		addNode(9, 7);
		addNode(9, 2);
		addNode(9, 1);
		addNode(10, 0);

		deleteNode(0);
		deleteNode(1);
		deleteNode(2);

		Node temp = head;
		while(temp.next != head) {
			System.out.format("%d ", temp.data);
			temp = temp.next;

			if(temp.next == head) {
				System.out.format("%d ", temp.data);
			}
		}

		System.out.format("\n%d", temp.next.data);
	}
}
```
