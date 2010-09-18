Simple set datatype for JavaScript
==========

This provides a set data type, with an API very close to that of [Python's sets module](http://docs.python.org/library/sets.html).

Installation
----------

If you have the node package manager, this is easy:

    npm install simplesets

You could also clone the git repository, and install it manually.

Usage
----------

Here's an example of how you use it:

    var sets = require('simplesets');
    
    var s1 = new sets.Set(['hello', 'world', 'how', 'are', 'you', 'today']);
    var s2 = new sets.Set(['say', 'hello', 'to', 'the', 'world']);
    
    // Print out both of the sets, as arrays of their elements.
    console.log('s1 =', s1.array());
    console.log('s2 =', s2.array());
    
    // Do some set operations.
    console.log('Intersection:', s1.intersection(s2).array());
    console.log('s1 - s2:', s1.difference(s2).array());
    console.log('s2 - s1:', s2.difference(s1).array());
    console.log('Union:', s1.union(s2).array());
    
    // Make a set with numbers and strings.
    s3 = new sets.Set([1, 2, 3, 'a', 'b', 'c']);
    console.log('Mixing data types:', s3.array());
    // Add in some more data types.
    var my_dict = {foo: 42, bar: 'bazaar'};
    s3.add(my_dict);		// This will add to the set...
    s3.add(my_dict);		// ...but now this will do nothing.
    s3.remove(3);
    s3.remove('c');
    console.log('New s3 =', s3.array());
    
    // You can make shallow copies of sets.
    s4 = new sets.Set([1, 2, 3]);
    s5 = s4.copy();
    s4.add(42);
    s5.remove(2);
    console.log('s4 =', s4.array());
    console.log('s5 =', s5.array());
    
The set data type has the simplest, stupidest implementation possible: an unordered array. This is because of how JavaScript's data types work. If it were possible to compute a hash value from any data type, or get its memory address, then we could do something more elaborate. If `<` and `>` operations were defined for all data types, we could use some kind of balanced tree representation, or sorted arrays. If JavaScript objects supported arbitrary data types as indices, this would all be too easy. But none of those things is true, so we're stuck relying only on the `===` operation, and unsorted arrays. For small sets, this is not a problem. For larger sets, if performance of set operations turns out to be problematic, you may want to make a specialized set data type. For example, if your set members are all strings, you could represent sets as objects with set members as keys, and it would be very fast. If enough people want this, I might build it into this library as well.

Set API
----------

The `Set` class has the following methods:

* `new Set(items)`: Creates a new set. If an array `items` is given, its contents will be added to the set.

* `has(x)`: Does this set contain an element `x`? Returns `true` or `false`.

* `add(x)`: Add an element `x` to this set, and return this set.

* `remove(x)`: Remove an element `x` from this set, if it is part of the set. If it is not part of the set, do nothing. Returns this set.

* `union(other)`: Return a new set containing the items found in either this set, the other set, or both.

* `intersection(other)`: Return a new set containing the items found in both this set and the other set.

* `difference(other)`: Return a new set containing the items in this set that are not in the other set.

* `symmetric_difference(other)`: Return a new set containing the items in either this set or the other set, but not both.

* `issubset(other)`: Return `true` if every element of this set is in the other set.

* `issuperset(other)`: Return `true` if every element of the other is in this set.

* `array()`: Return a copy of the items in the set, as an array.

* `size()`: Return the size of the set.

* `copy()`: Return a shallow copy of the set.

* `pop()`: Remove and return a random element of the set, or null if the set is empty.

The condition for determining whether two values are equal is the `===` operator. Therefore sets can support any mix of data types, as long as the data types can be compared for equality in some meaningful sense with `===`.
