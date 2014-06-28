##`Guava: Google Core Libraries for Java 1.6+`
This post covers the useful non complex utils provided by guava. Most of the content is copy-pasted from guava getting started guide
###Useful and not-so-complex utils
1. `Preconditions`: Test preconditions for your methods more easily.
2. `Common object methods`: Simplify implementing Object methods, like hashCode() and toString().
2. `Ordering`: Guava's powerful "fluent Comparator" class.
1. Collections: Guava's extensions to the JDK collections ecosystem. These are some of the most mature and popular parts of Guava.
  + `Immutable collections`, for defensive programming, constant collections, and improved efficiency.
  + New collection types, for use cases that the JDK collections don't address as well as they could: multisets, multimaps, tables, bidirectional maps, and more.
  + Powerful collection utilities, for common operations not provided in java.util.Collections.
  + Extension utilities: writing a Collection decorator? Implementing Iterator? We can make that easier.
1. `Strings`: A few extremely useful string utilities: splitting, joining, padding, and more.
2. `Ranges`: Guava's powerful API for dealing with ranges on Comparable types, both continuous and discrete.
3. `Hashing`: Tools for more sophisticated hashes than what's provided by Object.hashCode(), including Bloom filters.

###The ones that seemed complex
1. `Reflection`: Guava utilities for Java's reflective capabilities.
2. `EventBus`: Publish-subscribe-style communication between components without requiring the components to explicitly register with one another.
3. `Concurrency`: Powerful, simple abstractions to make it easier to write correct concurrent code.

###Preconditions
There are primarily 6 functions and 3 variants for each of these 6 functions
* No extra arguments
* An extra object whose toString method will be called and displayed in error message 
* An extra String argument, with an arbitrary number of additional Object arguments. This behaves something like printf, but for GWT compatibility and efficiency, it only allows %s indicators
Example
```java
checkArgument(i >= 0)
checkArgument(i >= 0, "Argument was %s but expected nonnegative", i);
checkArgument(i < j, "Expected i < j, but %s > %s", i, j);
```

The six core functions are

1. `checkArgument(boolean)`	Checks that the boolean is true. throws	`IllegalArgumentException`
2. `checkNotNull(T)` throws	`NullPointerException`
3. `checkState(boolean)`	Checks some state of the object `IllegalStateException`
4. `checkElementIndex(int index, int size), checkPositionIndex(int index, int size)` Check that index is a valid element index into a list, string, or array with the specified size. An element index may range from 0 inclusive to size exclusive, inclusive respectively. You don't pass the list, string, or array directly; you just pass its size. `IndexOutOfBoundsException`
5. `checkPositionIndexes(int start, int end, int size)` Same as above, but for the range `[start,end)` 
