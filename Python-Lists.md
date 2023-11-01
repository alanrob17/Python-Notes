# Python Lists

## List

Lists are used to store multiple items in a single variable.

Lists are one of 4 built-in data types in Python used to store collections of data, the other 3 are Tuple, Set, and Dictionary, all with different qualities and usage.

Lists are created using square brackets:

```python
    mylist = ["apple", "banana", "cherry"]
```

## List Items

List items are ordered, changeable, and allow duplicate values.

List items are indexed, the first item has index [0], the second item has index [1] etc.

### Ordered

When we say that lists are ordered, it means that the items have a defined order, and that order will not change.

If you add new items to a list, the new items will be placed at the end of the list.

**Note:** There are some list methods that will change the order, but in general: the order of the items will not change.

### Changeable

The list is changeable, meaning that we can change, add, and remove items in a list after it has been created.

### Allow Duplicates

Since lists are indexed, lists can have items with the same value:

```python
    mylist = ["apple", "banana", "cherry", "apple", "banana", "cherry"]

    for item in mylist:
        print(item)
```

Result:

> apple     
> banana        
> cherry        
> apple     
> banana        
> cherry        

The output shows that the order doesn't change. The items are in the order that they were entered.

## List Length

To determine how many items a list has, use the ``len()`` function:

```python
    thislist = ["apple", "banana", "cherry"]
    print(len(thislist))
```

> 3

## List Items - Data Types

List items can be of any data type:

```python
    list1 = ["abc", 34, True, 40, "male"]
```

## type()

From Python's perspective, lists are defined as objects with the data type 'list':

```python
    mylist = ["apple", "banana", "cherry"]
    print(type(mylist))
```

Returns:

> <class 'list'>

## The list() Constructor

It is also possible to use the ``list()`` constructor when creating a new list.

```python
    thislist = list(("apple", "banana", "cherry")) # note the double round-brackets
    print(thislist)
```

## Python Collections (Arrays)

There are four collection data types in the Python programming language:

* ``List`` is a collection which is ordered and changeable. Allows duplicate members.
* ``Tuple`` is a collection which is ordered and unchangeable. Allows duplicate members.
* ``Set`` is a collection which is unordered, unchangeable, and unindexed. No duplicate members.
* ``Dictionary`` is a collection which is ordered and changeable. No duplicate members.

**Set** items are unchangeable, but you can remove and/or add items whenever you like.

As of Python version 3.7, dictionaries are ordered.

When choosing a collection type, it is useful to understand the properties of that type. Choosing the right type for a particular data set could mean retention of meaning, and, it could mean an increase in efficiency or security.

## Access Items

List items are indexed and you can access them by referring to the index number:

```python
    thislist = ["apple", "banana", "cherry"]
    print(thislist[1])
```

> banana

**Note:** The first item has index 0.

## Negative Indexing

Negative indexing means start from the end

-1 refers to the last item, -2 refers to the second last item etc.

```python
    thislist = ["apple", "banana", "cherry"]
    print(thislist[-1])
```

> cherry

## Range of Indexes

You can specify a range of indexes by specifying where to start and where to end the range.

When specifying a range, the return value will be a new list with the specified items.

```python
    thislist = ["apple", "banana", "cherry", "orange", "kiwi", "melon", "mango"]

    print(thislist[2:5])

    newlist = thislist[1:5]

    print(newlist)
```

Returns:

> ['cherry', 'orange', 'kiwi']      
> ['banana', 'cherry', 'orange', 'kiwi']

By leaving out the start value, the range will start at the first item:

> ['apple', 'banana', 'cherry', 'orange']

By leaving out the end value, the range will go on to the end of the list:

```python
    thislist = ["apple", "banana", "cherry", "orange", "kiwi", "melon", "mango"]
    print(thislist[2:])
```

> ['cherry', 'orange', 'kiwi', 'melon', 'mango']

## Check if Item Exists

To determine if a specified item is present in a list use the in keyword:

```python
    thislist = ["apple", "banana", "cherry"]

    if "apple" in thislist:
        print("Yes, 'apple' is in the fruits list")
```

> Yes, 'apple' is in the fruits list

## Change Item Value

To change the value of a specific item, refer to the index number:

```python
    thislist = ["apple", "banana", "cherry"]
    thislist[1] = "blackcurrant"
    
    print(thislist)
```

> ['apple', 'blackcurrant', 'cherry']

## Change a Range of Item Values

To change the value of items within a specific range, define a list with the new values, and refer to the range of index numbers where you want to insert the new values:

```python
    thislist = ["apple", "banana", "cherry", "orange", "kiwi", "mango"]
    thislist[1:3] = ["blackcurrant", "watermelon"]

    print(thislist)
```

> ['apple', 'blackcurrant', 'watermelon', 'orange', 'kiwi', 'mango']

## Insert Items

To insert a new list item, without replacing any of the existing values, we can use the ``insert()`` method.

The ``insert()`` method inserts an item at the specified index:

```python
thislist = ["apple", "banana", "cherry"]
thislist.insert(2, "watermelon")
print(thislist)
```

> ['apple', 'banana', 'watermelon', 'cherry']

## Add List Items

### Append Items

To add an item to the end of the list, use the append() method:

```python
    thislist = ["apple", "banana", "cherry"]
    thislist.append("orange")

    print(thislist)
```

> ['apple', 'banana', 'cherry', 'orange']

### Insert Items

To insert a list item at a specified index, use the insert() method.

The ``insert()`` method inserts an item at the specified index:

```python
    thislist = ["apple", "banana", "cherry"]
    thislist.insert(1, "orange")
    
    print(thislist)
```

> ['apple', 'orange', 'banana', 'cherry']

## Extend List

To append elements from another list to the current list, use the ``extend()`` method.

```python
    thislist = ["apple", "banana", "cherry"]
    tropical = ["mango", "pineapple", "papaya"]
    thislist.extend(tropical)

    print(thislist)
```

> ['apple', 'banana', 'cherry', 'mango', 'pineapple', 'papaya']

### Add Any Iterable

The ``extend()`` method does not have to append lists, you can add any iterable object (tuples, sets, dictionaries etc.).

```python
    thislist = ["apple", "banana", "cherry"]
    thistuple = ("kiwi", "orange")
    thislist.extend(thistuple)

    print(thislist)
```

> ['apple', 'banana', 'cherry', 'kiwi', 'orange']

## Remove List Items

### Remove Specified Item

The ``remove()`` method removes the specified item.

```python
    thislist = ["apple", "banana", "cherry"]
    thislist.remove("banana")
    
    print(thislist)
```

> ['apple', 'cherry']

If there are more than one item with the specified value, the remove() method removes the first occurance:

```python
    thislist = ["apple", "banana", "cherry", "banana", "kiwi"]
    thislist.remove("banana")

    print(thislist)
```

> ['apple', 'cherry', 'banana', 'kiwi']

### Remove Specified Index

The ``pop()`` method removes the specified index.

```python
    thislist = ["apple", "banana", "cherry"]
    thislist.pop(1)

    print(thislist)
```

> ['apple', 'cherry']

If you do not specify the index, the ``pop()`` method removes the last item.

```python
    thislist = ["apple", "banana", "cherry"]
    thislist.pop()

    print(thislist)
```

> ['apple', 'banana']

The **del** keyword also removes the specified index:

```python
    thislist = ["apple", "banana", "cherry"]
    del thislist[0]

    print(thislist)
```

> ['banana', 'cherry']

The **del** keyword can also delete the list completely.

```python
    thislist = ["apple", "banana", "cherry"]
    del thislist
```

### Clear the List

The ``clear()`` method empties the list.

The list still remains, but it has no content.

```python
    thislist = ["apple", "banana", "cherry"]
    thislist.clear()
    
    print(thislist)
```

> []

## Loop through Lists

### Loop Lists

You can loop through the list items by using a for loop:

```python
    thislist = ["apple", "banana", "cherry"]
    
    for x in thislist:
      print(x)
```

> apple     
> banana        
> cherry

### Loop Through the Index Numbers

You can also loop through the list items by referring to their index number.

Use the ``range()`` and ``len()`` functions to create a suitable iterable.

```python
    thislist = ["apple", "banana", "cherry"]
    
    for i in range(len(thislist)):
      print(thislist[i])
```

> apple     
> banana        
> cherry

### Using a While Loop

You can loop through the list items by using a while loop.

Use the ``len()`` function to determine the length of the list, then start at 0 and loop your way through the list items by referring to their indexes.

Remember to increase the index by 1 after each iteration.

```python
    thislist = ["apple", "banana", "cherry"]
    i = 0
    
    while i < len(thislist):
      print(thislist[i])
      i = i + 1
```

> apple     
> banana        
> cherry

### Looping Using List Comprehension

List Comprehension offers the shortest syntax for looping through lists:

```python
    thislist = ["apple", "banana", "cherry"]

    [print(x) for x in thislist]
```

> apple     
> banana        
> cherry

## List Comprehension

List comprehension offers a shorter syntax when you want to create a new list based on the values of an existing list.

Based on a list of fruits, you want a new list, containing only the fruits with the letter "a" in the name.

Without list comprehension you will have to write a ``for`` statement with a conditional test inside:

```python
    fruits = ["apple", "banana", "cherry", "kiwi", "mango"]
    newlist = []

    for x in fruits:
      if "a" in x:
        newlist.append(x)

    print(newlist)
```

> ['apple', 'banana', 'mango']

With list comprehension you can do all that with only one line of code:

```python
    fruits = ["apple", "banana", "cherry", "kiwi", "mango"]
    
    newlist = [x for x in fruits if "a" in x]

    print(newlist)
```

> ['apple', 'banana', 'mango']

### The Syntax

newlist = [expression for item in iterable if condition == True]

The return value is a new list, leaving the old list unchanged.

#### Condition

The ``condition`` is like a filter that only accepts the items that valuate to True.

```python
    fruits = ["apple", "banana", "cherry", "kiwi", "mango"]
    
    newlist = [x for x in fruits if x != "apple"]

    print(newlist)
```

> ['banana', 'cherry', 'kiwi', 'mango']

The condition if **x != "apple"**  will return True for all elements other than "apple", making the new list contain all fruits except "apple".

The condition is optional and can be omitted:

```python
    fruits = ["apple", "banana", "cherry", "kiwi", "mango"]
    
    newlist = [x for x in fruits]

    print(newlist)
```

> ['apple', 'banana', 'cherry', 'kiwi', 'mango']

### Expression

The expression is the current item in the iteration, but it is also the outcome, which you can manipulate before it ends up like a list item in the new list:

Set the values in the new list to upper case:

```python
    fruits = ["apple", "banana", "cherry", "kiwi", "mango"]
    
    newlist = [x.upper() for x in fruits]

    print(newlist)
```

> ['APPLE', 'BANANA', 'CHERRY', 'KIWI', 'MANGO']

You can set the outcome to whatever you like:

```python
    fruits = ["apple", "banana", "cherry", "kiwi", "mango"]
    
    newlist = newlist = ['hello' for x in fruits]

    print(newlist)
```

> ['hello', 'hello', 'hello', 'hello', 'hello']

The expression can also contain conditions, not like a filter, but as a way to manipulate the outcome:

```python
    fruits = ["apple", "banana", "cherry", "kiwi", "mango"]
    
    newlist = [x if x != "banana" else "orange" for x in fruits]

    print(newlist)
```

> ['apple', 'orange', 'cherry', 'kiwi', 'mango']

## Sort Lists

### Sort List Alphanumerically

List objects have a ``sort()`` method that will sort the list alphanumerically, ascending, by default:

```python
    thislist = ["orange", "mango", "kiwi", "pineapple", "banana"]
    thislist.sort()

    print(thislist)
```

> ['banana', 'kiwi', 'mango', 'orange', 'pineapple']

Sort the list numerically:

```python
    thislist = [100, 50, 65, 82, 23]
    thislist.sort()
    
    print(thislist)
```

> [23, 50, 65, 82, 100]

### Sort Descending

To sort descending, use the keyword argument ``reverse = True``:

```python
    thislist = ["orange", "mango", "kiwi", "pineapple", "banana"]
    thislist.sort(reverse = True)

    print(thislist)

    thislist = [100, 50, 65, 82, 23]
    thislist.sort(reverse = True)

    print(thislist)
```

> ['pineapple', 'orange', 'mango', 'kiwi', 'banana']        
> [100, 82, 65, 50, 23]

Sort the list based on how close the number is to 50:

```python
    def myfunc(n):
      return abs(n - 50)

    thislist = [100, 50, 65, 82, 23]
    thislist.sort(key = myfunc)
    
    print(thislist)
```

> [50, 65, 23, 82, 100]

### Case Insensitive Sort

By default the ``sort()`` method is case sensitive, resulting in all capital letters being sorted before lower case letters:

Produces unexpected results.

```python
    thislist = ["banana", "Orange", "Kiwi", "cherry"]
    thislist.sort()

    print(thislist)
```

> ['Kiwi', 'Orange', 'banana', 'cherry']

Luckily we can use built-in functions as key functions when sorting a list.

So if you want a case-insensitive sort function, use str.lower as a key function:

```python
    thislist = ["banana", "Orange", "Kiwi", "cherry"]
    thislist.sort(key = str.lower)

    print(thislist)
```

> ['banana', 'cherry', 'Kiwi', 'Orange']

### Reverse Order

What if you want to reverse the order of a list, regardless of the alphabet?

The ``reverse()`` method reverses the current sorting order of the elements.

```python
    thislist = ["banana", "Orange", "Kiwi", "cherry"]
    thislist.reverse()

    print(thislist)

    thislist = [100, 50, 65, 82, 23]
    thislist.reverse()

    print(thislist)
```

> ['cherry', 'Kiwi', 'Orange', 'banana']        
> [23, 82, 65, 50, 100]

## Copy Lists

## Copy a List

You cannot copy a list simply by typing list2 = list1, because: list2 will only be a reference to list1, and changes made in list1 will automatically also be made in list2.

There are ways to make a copy, one way is to use the built-in List method ``copy()``.

```python
    thislist = ["apple", "banana", "cherry"]
    mylist = thislist.copy()

    print(mylist)
```

> ['apple', 'banana', 'cherry']

Another way to make a copy is to use the built-in method ``list()``.

```python
thislist = ["apple", "banana", "cherry"]
mylist = list(thislist)

print(mylist)
```

> ['apple', 'banana', 'cherry']

## Join Lists

There are several ways to join, or concatenate, two or more lists in Python.

One of the easiest ways are by using the + operator.

```python
    list1 = ["a", "b", "c"]
    list2 = [1, 2, 3]

    list3 = list1 + list2
    print(list3)
```

> ['a', 'b', 'c', 1, 2, 3]

Another way to join two lists is by appending all the items from list2 into list1, one by one:

```python
    list1 = ["a", "b" , "c"]
    list2 = [1, 2, 3]

    for x in list2:
      list1.append(x)

    print(list1)
```

> ['a', 'b', 'c', 1, 2, 3]

Or you can use the ``extend()`` method, where the purpose is to add elements from one list to another list:

```python
    list1 = ["a", "b" , "c"]
    list2 = [1, 2, 3]

    list1.extend(list2)
    print(list1)
```

> ['a', 'b', 'c', 1, 2, 3]

## List Methods

Python has a set of built-in methods that you can use on lists.

|Method	   |Description
|----------|-----------------------------------------------------------------------------|
|append()  |Adds an element at the end of the list                                       |  
|clear()   |Removes all the elements from the list                                       |
|copy()    |Returns a copy of the list                                                   |
|count()   |Returns the number of elements with the specified value                      |
|extend()  |Add the elements of a list (or any iterable), to the end of the current list |
|index()   |Returns the index of the first element with the specified value              |
|insert()  |Adds an element at the specified position                                    |
|pop()     |Removes the element at the specified position                                |
|remove()  |Removes the item with the specified value                                    | 
|reverse() |Reverses the order of the list                                               |
|sort()    |Sorts the list                                                               |
