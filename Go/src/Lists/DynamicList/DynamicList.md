# Dynamic List

## Introduction
A dynamic list is based on an array, which keeps growing in size. It's similar to the fixed list, but with the additional adavntage of having no upper limit about how many itms you can insert in it.

## Advantages
- Fast to load in the cache since elements are next to each other in memory.
- Transversal in both direction.
- Can access any element in O(1) time.

## Disadvantages
- Insertion and deletion is O(N).
- Wastage of space.

## Uses
- Shopping lists.
- std::vector and Vec! in C++ and Rust respectively.

## Implementation

```go
// Package doublylinkedlist - An implementation of a dynamic array list
// Copyright 2019, Mihir Wadekar and Chaitenya Gupta
package dynamiclist

// Element - A blank interface to allow list of all datatypes
type Element interface {
	Equal(index uint64) bool
}

// DynamicList - Struct to create a dynamic array list
type DynamicList struct {
	data []Element
	size uint64
}

func (l *DynamicList) Equal(index uint64, value Element) bool {
	return l.Get(index) == value
}

// Size - Returns the size of the list
func (l *DynamicList) Size() uint64 {
	return l.size
}

// Empty - Checks if the list is empty or not
func (l *DynamicList) Empty() bool {
	return l.size == 0
}

// Insert - Inserts an element at the given position in the list
func (l *DynamicList) Insert(value Element, index uint64) {
	if index == l.size {
		// If index is equal to the size of the list,
		// just append the value to the end of the slice
		l.data = append(l.data, value)
	}
	if index > l.size {
		// If given index is greater than the
		// size of the list, panic
		panic("Invalid index")
	} else {
		for index < l.size {
			// Start from the given index and copy the elements
			// over by 1 position to the right
			l.data[index] = l.data[index-1]
			index++
		}
		// Set the given value at the given index
		l.data[index] = value
	}
	// Increment the list's size
	l.size++
}

// Remove - Removes an element from the given position in the list
func (l *DynamicList) Remove(index uint64) {
	if l.Empty() {
		// If the list is empty, panic
		panic("List empty")
	}
	if index > l.size {
		// If given index is greater than the
		// size of the list, panic
		panic("Invalid index")
	} else {
		for index < (l.size - 1) {
			// Start from the given index and copy the elements
			// over by 1 position to the left
			l.data[index] = l.data[index+1]
			index++
		}
	}
	// Decrement the list's size
	l.size--
}

// Find - Returns the index at which the element is found
func (l *DynamicList) Find(value Element) int {
	index := 0
	for uint64(index) < l.size {
		// Iterate till we find the element
		if l.Equal(uint64(index), value) {
			// If element found, return its position
			return int(index)
		}
		index++
	}
	// If element not found, return -1
	return -1
}

// Contains - Checks if the element is in the list
func (l *DynamicList) Contains(value Element) bool {
	return l.Find(value) != -1
}

// Get - Returns the element at a particular index
func (l *DynamicList) Get(index uint64) Element {
	if index >= l.size {
		// If given index is greater than
		// size of the list, panic
		panic("Invalid index")
	}
	// Return the element stored at the particular index
	// in the internal array
	return l.data[index]
}

```