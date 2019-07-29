---
layout: post
title:  "이중 연결 리스트(DoublyLinkedList)"
date:   2019-07-15 22:29:00
author: Seungbeom Kim
categories: datastructure_algorithm
tags:	자료구조 알고리즘 이중연결리스트
sitemap :
  changefreq : daily
  priority : 1.0
---

### 이중 연결 리스트

연결 리스트는 현재 노드가 다음 노드의 주소를 가지고 있는 형태이기 때문에 이전으로 되돌아 갈 수 없다. 이중 연결 리스트는 노트가 이전 노드와 다음 노드의 주소를 가지고 있어서 양방향으로 이동할 수 있다.

```Java
public class DoublyLinkedList {
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
			temp.prev = null;
			head = temp;
		}
		else {
			Node temp = target.next.next;
			temp.prev = target;
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
			temp.prev = newNode;
			head = newNode;
		}
		else {
			temp = target.next;
			temp.prev = newNode;
			newNode.next = temp;
			newNode.prev = target;
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
			target.next.prev = target;
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
		while(temp != null) {
			System.out.format("%d ", temp.data);
			prev = temp;
			temp = temp.next;
		}

		System.out.println("\n");

		while(prev != null) {
			System.out.format("%d ", prev.data);
			prev = prev.prev;
		}
	}
}
```
