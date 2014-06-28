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

###Common object methods and Ordering
Guava provides a convinient way of defining hashCode, toString , compareTo functions. Go through the toString, compareTo functions of the Person class. Comparision chain is really a handy tool for defining compareTo funciton. The functions I found useful in Ordering are reverse, nullsFirst, nullsLast. If not for these functions this is not a great improvement over Comparable .

Some utils for equals are also provided. I did not find it useful.Dont try to test Person class for Sets because equals function is not defined
```java
import com.google.common.base.Function;
import com.google.common.base.Objects;
import com.google.common.collect.ComparisonChain;
import com.google.common.collect.Ordering;
import java.util.ArrayList;
import java.util.Collections;


class Person implements Comparable<Person> {
    public final String lastName;
    public final String firstName;
    public final int zipCode;

    public Person(String lastName, String firstName, int zipCode) {
        this.lastName = lastName;
        this.firstName = firstName;
        this.zipCode = zipCode;
    }

    public int compareTo(Person that) {
        //compare function does not expect null values
        //if null values are expected then use a comparator
        return ComparisonChain.start()
                .compare(this.lastName, that.lastName)
                .compare(this.firstName, that.firstName, 
                        Ordering.natural().nullsFirst().reverse())
                .compare(this.zipCode, that.zipCode)
                .result();
    }
    public static Ordering<Person> orderingByArea(){
        Ordering<Person> byLengthOrdering = Ordering.natural().
                nullsFirst().onResultOf(new Function<Person, Integer>() {
            @Override
            public Integer apply(Person input) {
                return input.zipCode;
            }
        });
        return byLengthOrdering;
    }
    @Override
    public String toString() {
        //avoid using addValue, give it meaningful name as for lastName/firstName
        return Objects.toStringHelper(this)
                .add("ln", lastName)
                .add("fn", firstName)
                .addValue(zipCode)
                .toString();
    }


    @Override
    public int hashCode(){
        return Objects.hashCode(lastName, firstName, zipCode);
    }

}

public class GuavaOrdering {
    public static void main(String[] args) {
        final Person p1 = new Person("Sai", "Suram", 500072);
        final Person p2 = new Person("Rand", "Rand", 500071);
        final Person noFirstName = new Person("Sai", null, 1000000);

        ArrayList<Person> list = new ArrayList<Person>(){{
            add(p1);
            add(p2);
            add(noFirstName);
        }};
        Collections.sort(list);
        System.out.println(list);
        Collections.sort(list, Person.orderingByArea());
        System.out.println(list);
    }

}

```
And the output would be
```java
[Person{ln=Rand, fn=Rand, 500071}, Person{ln=Sai, fn=Suram, 500072}, Person{ln=Sai, fn=null, 1000000}]
[Person{ln=Rand, fn=Rand, 500071}, Person{ln=Sai, fn=Suram, 500072}, Person{ln=Sai, fn=null, 1000000}]
```
