#Java ```java.util.Arrays``` and ```StringBuffer```

###Notable functions of Arrays class
+ AsList : return an arraylist of elements of the array
+ binarySearch
  + For primitives comparator cannot be used (for obvious reasons)
  + For array of Objects Comparators can be supplied as parameter
+ copyOf : returns a new array which is copy of the given array (similarly copyOfRange
+ hashCode, equals, toString : obvious from the name
+ deepHashCode, deepEquals : for multi-dimensional arrays 
+ deepToString : for multidimensional arrays
+ fill : fill the specified elements of the array with specified value
+ sort : as in the case of binarySearch, comparator cannot be used for primitives

###StringBuffer
Java Strings are immutable. For buliding strings, ```StringBuffer``` and ```StringBuilder``` classes can be used. However StringBuilder should not be used unless, thread safety is required.

###Sources
[Arrays util](http://docs.oracle.com/javase/7/docs/api/java/util/Arrays.html)
