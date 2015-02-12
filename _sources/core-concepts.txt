Core Concepts 
=============

Values 
------

* A value can be as small as a integer, or as large as a table with a million records.

* Values are created as the result of evaluating expressions.

* Every value has a *type* (a model for its storage). 

* Values can be included as literals in the code, letting the compiler infer the type. e.g.::

	  let
	     a = 42                // inferred type: Integer
	     b = 42.2              // inferred type: Float64
	     c = decimal 42.2      // explicit type: Decimal
	     d = "abc"             // explicit type: String
	     e = list 1 2 3 4 5 6  // explicit type: List


**Values are immutable by default**. Functions can operate on values, but cannot modify the values they receive. 

For example the function *append* used to append elements to a list, will receive a list-value as parameter and will return a *new* list-value with the data appended.

*Lists* and other data structures, are implemented as `Persistent Data Structures`_. A *full memory duplication* is not required to achieve *logical immutable values*. 

.. _Persistent Data Structures: http://en.wikipedia.org/wiki/Persistent_data_structure

Today Multi-core processors are the norm. To truly utilize all the power of a multi-core processor you need concurrent programming. By discouraging "mutation" or "change-in-place" of values, you eliminate data-races from concurrent programming. Immutable values are also safe to share between threads (safe because values are immutable). Simplifying concurrent programming is a huge gain even with the performance penalties of persistent data structures. See: http://www.gotw.ca/publications/concurrency-ddj.htm (2005)

Storages
--------

* A *storage* is an arbitrarily sized area of memory where you can store a value.
* The shape of a *storage* is defined by its *type*. Normally, a storage for a single item occupies a small amount of memory while a storage for a large list, occupies a large amount of memory.

Naming Storages
***************

A "**name**" is a symbol, an *identifier* associated with a storage. 

* Each *storage* has one and only one unique name.
* Two different *names* **cannot refer to the same storage at the same time**.
* **Each name owns its storage**

You can create and name a *storage* with the core-command "**let**". e.g.::

	let answer = 42         // create a storage, type:Int, value: 42, named "answer". 
	let c = decimal 42.2    // create storage, type:Decimal, value: 42.2, named "c". 

Types
-----

Atom Types
**********

*Atom* types hold values composed of *one single item*, normally treated as a unit, i.e:

* Byte or Int8 to Int64 
* Uint8 to Uint64 
* Int (architecture dependent, Int32 or Int64) 
* Big-Integer (arbitrarily large signed integer)
* Decimal and Big-Decimal
* Float32 and Float64
* String

As with all types, the type name is also the command to create a value of the type, e.g.::

    let
      a = Int64 42        // type: Int64, value: 42
      b = Float64 1234    // type: Float64, value: 1234.0
      c = Big-Decimal 1e6 // type: Big-Decimal, value: 1 million

List Type
*********

A List is a type of value, where the value is composed of *a sequence of items* (zero or more), where each "item" in the sequence can be a value of any type, i.e.: each item can be an *Atom*, a *Structure* or *another List*.

*List* constructor syntax is:

.. container:: syntax

   list Value...

e.g.::

    let xs = list 1 2 3
    let mixed-list = list "abc" 20.5 1 
    let recursive-list = list "abc" 1 2 (4 5 6 ("J" "K" "L") 8 9 10)
    let discounts = list 
                discount 10 "PROMO" 
                discount 20 "VIP"

You can handle list items by using *first* and *rest* (a.k.a. *car* and *cdr*). You can process the table sequentially or concurrently using *for-each*, *map*, *filter*, etc. "List" implements the *Sequence-Interface*, the basic interface to handle sequences of items.

In a *List*, an item can be *another List*. **A List is a** `Recursive Data Type`_.

Structure Type
**************


A *structure* is a type of value, where the value is composed of a **fixed number of fields**, each field of any type. You can use dot-access (a dot) to access fields within the structure. 

To declare a structure type, the syntax is:

.. container:: syntax
 
   struct Name (field field-Name ":" Type)...

When you declare a structure, as with all types, the struct name is also the command used to construct a value of the structure type, e.g.::

     struct discount
         field pct: decimal
         field code: string

     let d = discount 10 "PROMO"        

     print d.pct  //dot-access
     => 10

     print d
     => (discount 10 "PROMO")

In a *Structure*, a field can be *another structure* or a *list*. The *Structure* is also a `Recursive Data Type`_.

.. _Recursive Data Type: https://en.wikipedia.org/wiki/Recursive_data_type