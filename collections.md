# Collections Framework
A collections framework is a unified architecture for representing and manipulating collections. It contain Interfaces, Implementations, and Algorithms.

* It follows Facade pattern.


## Core collection interfaces
```
		Collection 					Map
			|						 |
   Set  List  Queue  Deque		 SortedMap
 	|
SortedSet
```

## Set
A `Set` is a `Collection` that cannot contain duplicate elements.

**note:** Set is backed by Map.

* Identify duplicates
	1. HashSet (HashMap): `a.hashCode() == b.hashCode() && (a == b || a.equals(b))`

	* Why we compare hashCode() first?
	Contracts of hashCode():
	1. hashCode() returns consistent value
	2. a.equals(b) => a.hashCode() == b.hashCode()
	3. a.hashCode() == b.hashCode() ≠> a.equals(b)

	* What if we have a hashCode() returns a constant
	HashSet reduces to a linked list.

#### HashSet
* elements are stored in a HashMap
* best performing implementation
* lookup time: O(1)
* insert / delete: O(1)
* by default, if 75% space have been occupied, HashSet will resize (double the size) and rehashing all elements.

#### TreeSet
Red-Black tree => order of values.
Each element has to implement Comparable.

* duplicate check: CompareTo() or Comparator

#### LinkedHashSet
Addition to HashSet, it adds double link which connect the previous and next elements in the array (insertion-order).

#### Use `Set` to remove duplicates
```java
Collection<Type> noDups = new HashSet<Type>(c);
```

* Preserve order
```java
Collection<Type> noDups = new LinkedHashSet<Type>(c);
```

#### Remove operation
`remove()` uses the same duplicate check, and returns a boolean indicating whether the element was present.


## List
Ordered collection (sequential)

`get(int index)`
`set(int index, E element)`

### ArrayList
Use array as internal data structure.
* lookup: O(1)
* insert / delete: O(n)

* It is more suitable for stack
* Stack *has a* ArrayList (Adepter)

### LinkedList
Use doubly-linked list as internal data structure
* Lookup: O(n)
* insert / delete: O(1)

* It is more suitable for queue
* LinkedList implements Queue, it *is a* Queue (Facade)

### Why LinkedList is still better than ArrayList in insert / delete even with a O(n) lookup time.
Because reading is much faster writing, in other words, r/w : 1/n is worst than r/w : n/1.

### ArrayList vs Vector
|   | ArrayList | Vector |  
|---|-----------|--------|  
| Thread-safe | No | Yes |  
| Resizing Rate | 50% | 100% |  


* If we have unlimited memory, what the maximum number of elements we can add into ArrayList?
2^31, because ArrayList.get(int index), 0 ~ 2^31-1 that is the total index we have.

### Range-View Operation
`subList(int fromIndex, int toIndex)` returns a `List` view which is backed up by the `List`, so changes in the former are reflected in the latter.


## Queue
Two forms of methods:

| Type of Operation | Throws exception | Returns special value |  
|-------------------|------------------|-----------------------|
| Insert | add(e) |	offer(e) |  
| Remove | remove()	| poll() |  
| Examine | element() |	peek() |  

* generally no `null` elements.
* same `equals()` and `hasCode()` as `Object`.


## Deque
Two forms of methods:

| Type of Operation | Throws exception | Returns special value |  
|-------------------|------------------|-----------------------|  
| Insert | addFirst(e)<br>addLast(e) | offerFirst(e)<br>offerLast(e) |  
| Remove | removeFirst()<br>removeLast() | pollFirst()<br>pollLast() |  
| Examine | getFirst()<br>getLast() | peekFirst()<br>peekLast() |  

* with additional: `removeFirstOccurence()` and `removeLastOccurence()`, that remove the specified element and return `true` if the element exists or `false` and remains the `Deque` unchanged.


## Map
* no duplicate keys, each key can map to at most on one value.

### TreeMap
sorted key.

### HashMap vs Hashtable vs ConcurrentHashMap (Java 5)
|   | HashMap | Hashtable | ConcurrentHashMap |  
|---|---------|-----------|-------------------|  
| Thread-safe | No | Yes | Yes |  
| Performance | Best | Worst | Good |  
| Allows null key<br> (only one) | Yes | No | No |  
| Iterator | fail-fast | fail-fast | fail-safe |  

* Why ConcurrentHashMap has better performance than Hashtable?
Because Hashtable is one lock for entire table, while concurrentHashMap has 16 segments as a hash table array, each of which has a lock.

### Set vs Map
keySet of a Map is the same type of Set
* HashMap.keySet() -> HashSet
* LinkedHashMap.keySet() -> LinkedHashSet
* TreeMap.keySet() -> TreeSet

```java
Map<K, V> map;
Set<K> keys = map.keySet();
Collection<V> values = map.values();

map.put(K, V)
V value = map.getValue(Key);
```


## Comparator vs Comparable (#4)
```java
public interface Comparable<V> {
	int compareTo(V anther);
}

public interface COmparator<V> {
	int compare(V a, V b);
}
```

***note: String is Comparable.***

### Collections.sort
```java
public static <T extends Comparable<? super T>> sort(List<T> list)
```
To use the Collections.sort, a class has to implement Comparable.

### Collections.shuffle
```java
public static void shuffle(List<?> list, Random rnd) {
	for (int i = 0; i < list.size(); i++) {
		swap(list, i-1, rnd.nextInt(i));
	}
}
```

## SortedSet
SortedSet inherits from Set.

1. iterator traverse in order.
2. `toArray` contains sorted elements.

### Range View Operation
Different to list`s range-view, changes to the range-view write back to the backing sorted set and vice versa.


## SortedMap
### Range View Operation


## Iterator vs Enumeration
### Iterator
Traverse through a collection and remove elements selectively.

```java
public interface Iterator<E> {
	boolean hasNext();
	E next();
	void remove();
}
```

* `remove()` is the only safe way to modify a collection during iteration. It removes the last element that was returned by next. It can be used to filer an arbitrary `Collection`.

```java
static void filter(Collection<?> c) {
	for (Interator<?> it = c.iterator(); it.hasNext();)
		if (!cond(it.next()))
			it.remove();
}
```

#### ListIterator
with additional `hasPrevious()`, `previous()`, 'set()', 'add()'.

* backward iteration
```java
for (ListIterator<Type> it = list.Iterator(list.size()); it.hasPrevious(); ) {
	Type t = it.previous();
}
```

#### Fail-Fast Iterator
For a fail-fast iterator, after the creation of it, 
if the collection is structurally modified (insert, delete, update) at any time, in any way (other methods, other threads) except through iterator's `remove()` method, it will throw a `ConcurrentModificationException`.

**note:** Internally, the data structure maintains an `mods` flag.
**note:** Fail-fast behavior of an iterator cannot be guaranteed. This behavior *should be used only to detect bugs*.

#### Fail-Safe Iterator
Iterator first make a copy of the internal data structure and then iterates on copied one. The iterator will not reflect additions, removals, or changes to the collection since the iterator was created.

**note:** Element-changing operations on iterators themselves (remove(), set(), and add()) are not supported. These methods throw `UnsupportedOperationException`.

|  | Fail Fast Iterator | Fail Safe Iterator |  
|--|--------------------|--------------------|  
| Throw<br> ConcurrentModification<br> Exception<br> | Yes | No |  
| Clone | object | No | Yes |  
| Memory Overhead | No | Yes |  
| Element-changing operations | Yes | No |  
| Examples | HashMap,Vector,ArrayList,HashSet |  CopyOnWriteArrayList,ConcurrentHashMap |  

### Enumeration
```
public interface Enumeration<V> {
	V nextElement();
	boolean hasMoreElements();
}
```

### How to convert non-thread-safe Collection to thread-safe.
`{NAME}	synchronized{NAME} (NAME collection);`  
NAME: Collection, List, Set, Map, SortedSet, SortedMap

This method returns a SynchronizedCollection<E> which contains an `final Object mutex` and wraps almost all functions in a `synchronized(mutex) { ... }` block in addition to the original Collection.

```java
List<V> list = new ArrayList<V>();
List<V> list2 = Collections.synchronizedList(list);

list != list2
```

**note:** synchronizedList is still better than vector, because synchronized version allows multi-thread reading. Vector only allows single thread access.

### How to make a collection read-only
`{NAME} unmodifiable{NAME} (NAME collection);`  
NAME: Collection, List, Set, Map, SortedSet, SortedMap


