<!-- contributed by Irakli Gozalishvili [gozala@mozilla.com]  -->

The `"list"` module provides base building blocks for composing lists.

Reference
---------
<api name="Iterable">
@constructor
Base trait that can be used to compose traits with non-standard
enumeration behaviors.
</api>
<api name="List">
@constructor
Constructor can takes any number of element and creates instance of
`List` populate with specified elements.
@param [element1] {Object|String|Number}
@param [element2] {Object|String|Number}
@param [...] {Object|String|Number}
</api>
**Examples:**

    const MyList = List.compose({
      add: function add(item1, item2, /*item3...*/) {
        Array.slice(arguments).forEach(this._add.bind(this));
      },
      remove: function remove(item1, item2, /*item3...*/) {
        Array.slice(arguments).forEach(this._remove.bind(this));
      }
    });
    MyList('foo', 'bar', 'baz').length == 3;        // true
    new MyList('new', 'keyword').length == 2;       // true
    MyList.apply(null, [1, 2, 3]).length == 3;      // true
    let list = MyList();
    list.length == 0;                               // true
    list.add(1, 2, 3) == 3;                         // true

Iterable
--------
Trait is supposed to be used as part of a composition, since it only provides
custom enumeration behavior to a composed object.
It defines one required `_keyValueMap` property, that is used as a hash of
"key-values" to iterate on during enumeration.

**_Required properties_**

---
<api name="_keyValueMap">
@property {Object}
Hash map of key-values to iterate over.  
_Note: That this property can be a getter if you need dynamic behavior._
</api>



List
----

An ordered collection (also known as a sequence) disallowing duplicate
elements. List is composed out of `Iterable`, there for it provides custom
enumeration behavior that is similar to array (enumerates only on the
elements of the list). List is a base trait and is meant to be a part of
composition, since all of it's API is private except length property.

**_Public properties_**

---

<api name="length">
@property {Number}
Number of elements in this list.
</api>

**_Private properties_**

---

<api name="_has">
@method
@param element {Object|Number|String}
Returns `true` if this list contains the specified `element`.
</api>
<api name="_add">
@method
@param element {Object|Number|String}
Appends the specified `element` to the end of this list, if it doesn't
contains it.

_Ignores the call if `element` is already contained._
</api>
<api name="_remove">
@method
@param element {Object|Number|String}
Removes specified `element` from this list, if it contains it.

_Ignores the call if `element` is not contained._
</api>
<api name="_clear">
@method
Removes all of the elements from this list.
</api>

