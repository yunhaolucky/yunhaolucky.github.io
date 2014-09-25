---
layout: post
title:  "Java Collection"
date:   2014-09-10 09:22:44
categories: Interview
---

#Java collection
## Colletion
* `Collection` interface entends `Iterable`.
  * `Iterable` interface has only one method `Iterator<> iterator`
  * `Iterable` use the new while loop `for( type tree : forest){}`
* `Colletion`
  * There is no usable Implementation of `Collection`
  * Standard Methods:
    * add(`element`)
    * addAll(`collection`)
    * remove(`element`)
    * removeAll(`collection`)
    * contains(`element`)
    * containsAll(`collection`)
    * size()
    * sort( )
  * Iterating a collection

    ```java
    Collection c = new Set();
    Iterator i = c.iterator();
    while(i.hasNext()){
      Object o = i.next();
      }
    # or
    for(object o :c){
      object a = o;
    }

    ```

### List
* `list` interface is a subtype of `Collection` interface
* Implementation
  * `ArrayList`
  * `LinkedList`
  * `Vector`
  * `Stack`
* method
  * All methods of `Collection`
  * add(`index`, `element`)
  * get(`index`)
  * :question: remove(`index`) (besides remove(`element`))

### Set
* `Set` interface is also a subtype of `Collection` interface
* Implementation
  * `EnumSet` (Basic Type)
  * `HashSet`
  * `LinkedHashSet`
    * guarantee the order of the elements
  * `TreeSet`
    * sorting order of set
* Method
  * All method of `collection`
* Subtype
  * SortedSort
  * NavigableSort

###Map
* "NOT" a subtype of `Collection`
* a interface represents a mapping between a key and a value
* Implementation
  * :sunny:HashMap
  * Hashtable
  * EnumMap
  * IdentityHashMap
  * IdentityHashMap
  * LinkedHashMap
  * Properties
  * TreeMap
  * WeekHashMap
* Methods
  * :sunny: put(`key`, `value`)
  * :sunny: get('key')
  * :sunny: keySet() (important for iterator)
  * :sunny: remove(`key`)
* Subtype
  * SortedMap
  * NavigatiableMap

###Queue
* `Queue` is a subtype of `Collection`
* Implementation
  * :sunny:LinkedList
  * :sunny: PriorityQueue
* Method:
  * All method of `Collection`
*Subset:
  * Deque
    * method:
      * addFirst()
      * addLast()
      * getFirst()
      * getLast()
      * removeFirst()
      * removeLast()


#####stack
* method
  * push()
  * peek()
  * pop()
