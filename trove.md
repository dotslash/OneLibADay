#[GNU Trove](http://trove4j.sourceforge.net/html/overview.html)
1. [Maps between primitivies and Objects, Objects and primitives](https://github.com/dotslash/OneLibADay/blob/master/trove.md#primitive-collections)
2. [Faster Maps and Sets](https://github.com/dotslash/OneLibADay/blob/master/trove.md#faster-maps-and-sets)
3. [Utils for Maps, Sets](https://github.com/dotslash/OneLibADay/blob/master/trove.md#utils-for-maps-sets)

###Primitive Collections
Java Collections are inefficient for primitives as they do unnecessary conversions between primitives and their Object coutner parts. Grove provides Maps, Sets, Lists which use primitives. For Sets and Maps only hashed collections are provided.

* ```T{primitive}Object{MapType}<T>``` : example TIntObjectHashMap 
* ```TObject{primitive}{MapType}<T>``` : example TObjectIntHashMap
* ```T{primitive}{primitive}{MapType}``` : example TIntFloatHashMap
* ```T{primitive}{ListType/SetType/StackType}``` : example TIntHashSet, TLongArrayStack, TLongArrayList, TLongLinkedList

The usage of these collections is pretty same as of normal maps and sets. These collections offer good performance over standard java collections

###Faster Maps and Sets
The standard java collection's Implementation of Maps and Sets are backed by Maps. That means a TreeSet is actually a map internally.
Java Maps have an inefficiency. To support the entrySet, keySet functionality, the collections implementations do something more and make them inefficient. Trove Maps and Sets do have this fucntionality. As long as entrySet is not called Trove collections will be significantly better than Java collections. [Im not sure about the details](#). Instead the ```forEachKey``` ```forEachEntry``` ```forEachValue``` functions should cover most of the use cases

* ```THashMap<K,V>```
* ```THashSet<T>```

###Utils for Maps, Sets
For Maps
```java
//assume TDoubleIntHashMap
int putIfAbsent( double key, int val )
public key adjustOrPutValue( double key, int adjust_amount, int put_amount )
public boolean increment( double key )
public boolean adjustValue( double key, int amount )
```
For both Sets and Maps
```java
boolean retainEntries( TDoubleIntProcedure tDoubleIntProcedure )
public void compact() //rehash and make the collection compact
```
Custom Hash function
HashMaps, HashSets can be created with custom hashfucntions. A HashingStrategy object should be created and passed as parameter to the Map/Set contructor.
HashingStrategy object defines equals and hash function needed for trove hashset
```java
final TObjectIntCustomHashMap<String[]> map = 
        new TObjectIntCustomHashMap<String[]>( new HashingStrategy<String[]>() {
    @Override
    public int computeHashCode( String[] array ) {
        return Arrays.hashCode( array );
    }

    @Override
    public boolean equals( String[] ar1, String[] ar2 ) {
        return Arrays.equals( ar1, ar2 );
    }
});
```
