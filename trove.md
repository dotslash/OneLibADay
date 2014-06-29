#[GNU Trove](http://trove4j.sourceforge.net/html/overview.html)
1. Maps between primitivies and Objects, Objects and primitives
2. Faster Maps and Sets 
3. Utils for Maps, Sets

###Primitive Collections
Java Collections are inefficient for primitives as they do unnecessary conversions between primitives and their Object coutner parts. Grove provides Maps, Sets, Lists which use primitives. For Sets and Maps only hashed collections are provided.

* ```T{primitive}Object{MapType}``` : example TIntObjectHashMap 
* ```TObject{primitive}{MapType}``` : example TObjectIntHashMap
* ```T{primitive}{primitive}{MapType}``` : example TIntFloatHashMap
* ```T{primitive}{ListType/SetType/StackType}``` : example TIntHashSet
