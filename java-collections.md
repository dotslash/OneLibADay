Java utils collections
==

###Interfaces
The Java Collections Framework hierarchy consists of two distinct interface trees [[1]](http://docs.oracle.com/javase/tutorial/collections/interfaces/summary.html)

+ The first tree starts with the Collection interface, which provides for the basic functionality used by all collections, such as add and remove methods. Its subinterfaces — Set, List, and Queue — provide for more specialized collections.
+ The Set interface does not allow duplicate elements. This can be useful for storing collections such as a deck of cards or student records. The Set interface has a subinterface, SortedSet, that provides for ordering of elements in the set.
+ The List interface provides for an ordered collection, for situations in which you need precise control over where each element is inserted. You can retrieve elements from a List by their exact position.
+ The Queue interface enables additional insertion, extraction, and inspection operations. Elements in a Queue are typically ordered in on a FIFO basis.
+ The Deque interface enables insertion, deletion, and inspection operations at both the ends. Elements in a Deque can be used in both LIFO and FIFO.
+ The second tree starts with the Map interface, which maps keys and values similar to a Hashtable.
+ Map's subinterface, SortedMap, maintains its key-value pairs in ascending order or in an order specified by a Comparator.

These interfaces allow collections to be manipulated independently of the details of their representation.


###Implementations
HashSet, HashMap, TreeSet, TreeMap, ArrayList are very well known. The other Collections to look for are

+ LinkedList : implements  Deque, List and Queue
+ PriorityQueue : Implementation of standard priority queue. Needs a comparator or the elements shoudl have a natural order
+ ArrayDeque : Most operations run in amortized constant time. Exceptions include remove, removeFirstOccurrence, removeLastOccurrence, contains, iterator.remove(), and the bulk operations, all of which run in linear time.
+ Unmodifiable Wrappers : each of the six core Collection interfaces has one static factory method which returns an unmodifiable Collection. Collections class has static functions unmodifiable{X}. X could be one of Set, Map, SortedSet, SortedMap, List, Collection.
