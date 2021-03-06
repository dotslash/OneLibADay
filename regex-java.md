Java Regular Expressions
===

+ [Basics](#basics)
+ [Syntax](#syntax)
+ [Capturing Patterns](#capturing-patterns)
+ [Searching Matches](#searching-matches)
+ [Replace](#replace)
  + [Using patterns in replace](#using-patterns-in-replace)
+ [Lesson on Regular Expressions by Oracle](http://docs.oracle.com/javase/tutorial/essential/regex/index.html)

### Basics
+ ```Pattern``` : a compiled representation of a regular expression. To create a pattern, you must first invoke one of its public static compile methods, which will then return a Pattern object.
+ ```Matcher``` : interprets the pattern and performs match operations against an input string. Matcher object  can be obtained by invoking the matcher method on a Pattern object.

###Syntax

####Patterns to match a single character

| <b>pattern</b> | description                                              |
|----------------|----------------------------------------------------------|
| [abc]        | a, b, or c (simple class)                                |
| [^abc]         | Any character except a, b, or c (negation)               |
| [a-zA-Z]       | a through z, or A through Z, inclusive (range)           |
| [a-d[m-p]]     | a through d, or m through p: \[a-dm-p\] (union)            |
| [a-z&&[def]]   | d, e, or f (intersection)                                |
| [a-z&&[^bc]]   | a through z, except for b and c: \[ad-z\] (subtraction)    |
| [a-z&&[^m-p]]  | a through z, and not m through p: \[a-lq-z\] (subtraction) |

####Standard special aliases for character matching

|special patterns|description|
|----|-----|
| .              | Any character (may or may not match line terminators)    |
| \d             | A digit: [0-9]                                           |
| \D             | A non-digit: [^0-9]                                      |
| \s             | A whitespace character: [ \t\n\x0B\f\r]                  |
| \S             | A non-whitespace character: [^\s]                        |
| \w             | A word character: [a-zA-Z_0-9]                           |
| \W             | A non-word character: [^\w]                              |

####Qunatifiers

|   quantifiers    |  description                   |
|---|---|
|X?	    | once or not at all|
|X*	    | zero or more times|
|X+	    | one or more times|
|X{n}	  |	X, exactly n times|
|X{n,}	|	X, at least n times|
|X{n,m}	| X, at least n but not more than m times|
|(?!X) | is the pattern before the present point(look behind)|

####Boundary Matchers

|Boundary Construct |	Description |
|---|---|
|^	| The beginning of a line|
|$	| The end of a line|
|\b	| A word boundary|
|\B	| A non-word boundary|
|\A	| The beginning of the input|
|\G	| The end of the previous match|
|\Z	| The end of the input but for the final terminator, if any|
|\z	| The end of the input|


###Capturing Patterns
Capturing groups are numbered by counting their opening parentheses from left to right. In the expression ((A)(B(C))), for example, there are four such groups:

1. ((A)(B(C)))
2. (A)
3. (B(C))
4. (C)

The entire sequence is considered as match #0. Patters inside a (?..) are not captured

###Searching Matches

+ lookingAt(): Attempts to match the input sequence, starting at the beginning of the region, against the pattern.
+ matches(): Attempts to match the entire region against the pattern.
+ find(): Attempts to find the next subsequence of the input sequence that matches the pattern.
+ find(int start): Resets this matcher and then attempts to find the next subsequence of the input sequence that matches the pattern, starting at the specified index.

group, start, end are the other important functions to look for. Here is an example.
```java
public static void patterns() {
    Pattern pattern = Pattern.compile("(a+)(?!a)b(b)");
    Matcher matcher = pattern.matcher("aaabbbabb");
    while (matcher.find()) {
        System.out.println(matcher.group());
        for (int i = 0; i <= matcher.groupCount(); i++) {
            System.out.printf("group number: %d match: %s start: %d end: %d\n", 
                    i, matcher.group(i), 
                    matcher.start(i), matcher.end(i));
        }
        System.out.println();
    }
}
```
The output is as follows

```
aaabb
group number: 0 match: aaabb start: 0 end: 5
group number: 1 match: aaa start: 0 end: 3
group number: 2 match: b start: 4 end: 5

abb
group number: 0 match: abb start: 6 end: 9
group number: 1 match: a start: 6 end: 7
group number: 2 match: b start: 8 end: 9
```

###Replace
+ matcher.replaceAll : returns a new string replacing all the matches with the replacement string
+ matcher.replaceFirst : returns a new string replacing first the matches with the replacement string

String class too has equivalent functions which do the same. 

####Using patterns in replace

The $ symbol can be used to reference pattern matches. Here is an example
```java
public static void dateFormat() {
    String dateYMD = "2014-06-30";
    Matcher matcher = Pattern.compile("(\\d{4})-(\\d{2})-(\\d{2})").matcher(dateYMD);
    String dateDMY = matcher.replaceAll("$3-$2-$1");
    String dateMDY = dateYMD.replace("(\\d{4})-(\\d{2})-(\\d{2})","$2-$1-$3");
    System.out.printf("dateDMY : %s\ndateMDY : %s\n", dateDMY, dateMDY);
}
```
Out put will be as follows
```
dateDMY : 30-06-2014
dateMDY : 2014-06-30
```
