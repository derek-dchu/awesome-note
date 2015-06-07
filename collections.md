# Collections Framework
A collections framework is a unified architecture for representing and manipulating collections. It contain Interfaces, Implementations, and Algorithms.

* It follows Facade pattern.


## Core collection interfaces
```
		Collection 			     Map
			|				        |
   Set  List  Queue  Deque       SortedMap
    |
SortedSet
```

```java
public interface Collection<E> extends Iterable<E> {
    //method definitions
}
```


## Set
A `Set` is a `Collection` that cannot contain duplicate elements.

**Note:** Set is backed by Map.

### Identify duplicates
*  `a.hashCode() == b.hashCode() && (a == b || a.equals(b))`

*  Why we compare `hashCode` first?  
Contracts of `hashCode()`:

	1. hashCode() returns consistent value
	2. a.equals(b) => a.hashCode() == b.hashCode()
	3. a.hashCode() == b.hashCode() ≠> a.equals(b)

*  If `hashCode()` returns a constant: HashSet reduces to a linked list.

#### HashSet
*  Elements are stored in a `HashMap`.
*  Best performing implementation.
*  Lookup time: O(1).
*  Insert / Delete: O(1).
*  By default, if 75% space have been occupied, HashSet will resize (double the size) and rehashing all elements.
*  One `null` element is allowed.

#### TreeSet
*  Elements are stored in a `NavigableMap`.
*  Red-Black tree => order of elements.
*  Each element has to implement Comparable.
*  Duplicate check: CompareTo() or Comparator.
*  `null` element is not allowed.

#### LinkedHashSet
Addition to HashSet, it adds double link which connect the previous and next elements in the array (insertion-order).

#### Use `Set` to remove duplicates
```java
Collection<Type> noDups = new HashSet<Type>(c);
```

If we need to preserve order
```java
Collection<Type> noDups = new LinkedHashSet<Type>(c);
```

#### Remove operation
`remove()` uses the same duplicate check, and returns a boolean indicating whether the element was present.


## List
Ordered collection (sequential)

*  `get(int index)`
*  `set(int index, E element)`

### ArrayList
Use array as internal data structure.
*  Lookup: O(1)
*  Insert / Delete: O(n)


*  It is more suitable for stack
*  Stack *has a* ArrayList (Adepter)

#### ArrayList vs Vector
|   | ArrayList | Vector |  
|---|-----------|--------|  
| Thread-safe | No | Yes |  
| Resizing Rate | 50% | 100% |  


#### If we have unlimited memory, what the maximum number of elements we can add into ArrayList?
`Integer.MAX_VALUE` - x, where x is differed based on VM limits. Because ArrayList.get(int index) uses an `int` as index. And actually, when we new an array with length `Integer.MAX_VALUE` - x, it throws an `java.lang.OutOfMemoryError` exception.

### LinkedList
Use doubly-linked list as internal data structure
*  Lookup: O(n)
*  Insert / Delete: O(1)


*  It is more suitable for queue
*  LinkedList implements Queue, it *is a* Queue (Facade)

### Why LinkedList is still better than ArrayList in insert / delete even with a O(n) lookup time.
Because reading is much faster writing, in other words, r/w : 1/n is worst than r/w : n/1.

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
An implementation of SortedMap. Sorted key.

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

### How to design a key for HashMap
We can use custom class as the key for HashMap, however we may also face the problem that after an insertion, fields in key changed which leads the hash code of key is also changed, so we can never get back the value using this key.

That is reason why we use Integer, String which by default are immutable in general. In other words, a good key for HashMap has to have a consistent hash code.


## Comparator vs Comparable (#4)
```java
public interface Comparable<V> {
	int compareTo(V anther);
}

public interface COmparator<V> {
	int compare(V a, V b);
}
```

**Note:** String is Comparable.

### The `Collections.sort` method
```java
public static <T extends Comparable<? super T>> sort(List<T> list)
public static <T extends Comparable<? super T>> sort(List<T> list, Comparator<? super T> c)
```
To use the `Collections.sort()`, a class has to implement Comparable.

### The Collections.shuffle() method
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

*  `remove()` is the only safe way to modify a collection during iteration. It removes the last element that was returned by next. It can be used to filer an arbitrary `Collection`.

    ```java
    static void filter(Collection<?> c) {
    	for (Interator<?> it = c.iterator(); it.hasNext();)
    		if (!cond(it.next()))
    			it.remove();
    }
    ```

#### ListIterator
with extra methods: `hasPrevious()`, `previous()`, `set()`, `add()`.

* backward iteration
```java
for (ListIterator<Type> it = list.Iterator(list.size()); it.hasPrevious(); ) {
	Type t = it.previous();
}
```

#### Fail-Fast Iterator
For a fail-fast iterator, after the creation of it, 
if the collection is structurally modified (insert, delete, update) at any time, in any way (other methods, other threads) except through iterator's `remove()` method, it will throw a `ConcurrentModificationException`.

**Note:** 
*  Internally, the data structure maintains an `mods` flag.
*  Fail-fast behavior of an iterator cannot be guaranteed. This behavior *should be used only to detect bugs*.

#### Fail-Safe Iterator
Iterator first make a copy of the internal data structure and then iterates on copied one. The iterator will not reflect additions, removals, or changes to the collection since the iterator was created.

**Note:** Element-changing operations on iterators themselves (remove(), set(), and add()) are not supported. These methods throw `UnsupportedOperationException`.

|  | Fail Fast Iterator | Fail Safe Iterator |  
|--|--------------------|--------------------|  
| Throw<br> ConcurrentModification<br> Exception<br> | Yes | No |  
| Clone object | No | Yes |  
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

This interface is duplicated by the `Iterator` interface for shorter name and additional operation (remove). New implementations should consider using `Iterator` in preference to `Enumeration`.

### How to convert non-thread-safe Collection to thread-safe.
`{NAME}	synchronized{NAME} (NAME collection);`  
NAME: Collection, List, Set, Map, SortedSet, SortedMap

This method returns a SynchronizedCollection<E> which contains an `final Object mutex` and wraps almost all functions in a `synchronized(mutex) { ... }` block in addition to the original Collection.

```java
List<V> list = new ArrayList<V>();
List<V> list2 = Collections.synchronizedList(list);

list != list2
```

**Note:** synchronizedList is still better than vector, because synchronized version allows multi-thread reading. Vector only allows single thread access.

### How to make a collection read-only
`{NAME} unmodifiable{NAME} (NAME collection);`  
NAME: Collection, List, Set, Map, SortedSet, SortedMap

*  It is backed by original collection, there for any change on original collection can be seen in this collection.


