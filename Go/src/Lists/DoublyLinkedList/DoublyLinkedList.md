# Doubly Linked List

![A doubly linked list node](https://ucarecdn.com/f7059e4b-907f-4c4d-b156-6f437ddd7588/)

## Introduction
A doubly linked list is pretty similar to a singly linked list, with each node having an additional link to its previous nodes, enabling us to reversely iterate over the list.

Having this extra link, enables us to cut in half, the time complexity of singly linked lists. For example, we can optimize the time required to search for an element in the linked list by iterating from both sides at the same time.

## Advantages
- No space is wasted
- Really easy to implement
- We can traverse in both directions

## Disadvantages
- Numerous pointer operations slow down insertion, deletion and iteration.
- Have to maintain an extra field as compared to singly linked lists.

## Uses
- Browser forward and backward navigation.
- Undo and Redo functionality.
- Music player navigation, previous and next song.

## Implementation

```go
// Package doublylinkedlist - An implementation of a doubly linked list
// Copyright 2019, Mihir Wadekar and Chaitenya Gupta

package doublylinkedlist

// Element - A blank interface to allow list of all datatypes
type Element interface{}

// Node -  Struct to store a value in the doubly linked list
type Node struct {
	prev  *Node
	next  *Node
	value Element
}

// DoublyLinkedList - Struct to create a doubly linked list
type DoublyLinkedList struct {
	head *Node
	tail *Node
	size uint64
}

// Size - Returns the size of the list
func (l *DoublyLinkedList) Size() uint64 {
	return l.size
}

// Empty - Checks if the list is empty or not
func (l *DoublyLinkedList) Empty() bool {
	return l.size == 0
}

// getNode - Helper function to get the node at a particular index
func (l *DoublyLinkedList) getNode(index uint64) *Node {
	if index >= l.size {
		// If given index is greater than or equal to the
		// size of the list. return nil
		return nil
	}
	// Set n as the head initially
	n := l.head
	for index > 0 {
		// Set n to its next node and iterate untill we reach
		// the node at the given position
		n = n.next
		index--
	}
	// Return the node at the given position
	return n
}

// Insert - Inserts an element at the given position in the list
func (l *DoublyLinkedList) Insert(value Element, index uint64) {
	if index > l.size {
		// If the given index is bigger than the
		// size of the list, panic
		panic("Can't insert at invalid index")
	}

	n := &Node{value: value}
	if l.head == nil && l.tail == nil {
		// If the head and tail are nil, set them to n
		l.head = n
		l.tail = n
	} else {
		// Else, get the node at the previous index, and change
		// the current and previous nodes' field
		prev := l.getNode(index - 1)
		(*n).next = prev.next
		(*n).prev = prev
		prev.next = n
	}
	// Increment the size
	l.size++
}

// Remove - Removes an element from the given position in the list
func (l *DoublyLinkedList) Remove(index uint64) {
	if index > l.size {
		// If given index is greater than the
		// size of the list, panic
		panic("Can't remove from invalid index")
	}

	if l.size == 0 {
		// If list is empty, panic
		panic("Can't remove from empty list")
	} else if l.size == 1 {
		// If size of the list was 1, set
		// the head and tail to nil
		l.head = nil
		l.tail = nil
	} else {
		// Else, get the node at the previous index, and change
		// the current and previous nodes' field
		prev := l.getNode(index - 1)
		next := l.getNode(index + 1)
		prev.next = next
		next.prev = prev
	}
	l.size--
}

// Find - Returns the index at which the element is found
func (l *DoublyLinkedList) Find(value Element) int {
	index := 0
	for uint64(index) < l.size {
		// Iterate till we find the element
		n := l.getNode(uint64(index))
		if n.value == value {
			// If element found, return its position
			return int(index)
		}
		index++
	}
	// If element not found, return -1
	return -1
}

// Contains - Checks if the element is in the list
func (l *DoublyLinkedList) Contains(value Element) bool {
	return l.Find(value) != -1
}

// Get - Returns the element at a particular index
func (l *DoublyLinkedList) Get(index uint64) Element {
	if index >= l.size {
		// If given index is greater than
		// size of the list, panic
		panic("Can't get element at invalid index")
	}
	// Get the node at index and return its value
	return l.getNode(index).value
}

```
