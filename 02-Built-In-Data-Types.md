# 02 - Built-In Data Types

Data can be simple, whether it is an integer number to represent an age, or complex, like an order placed on a website. It can be about a single object or about a collection of them. Data can even be about data—that is, metadata. This is data that describes the design of other data structures, or data that describes application data or its context. In Python, objects are our abstraction for data, and Python has an amazing variety of data structures that you can use to represent data or combine them to create your own custom data.

we are going to cover the following:

* Python objects' structures
* Mutability and immutability
* Built-in data types: numbers, strings, dates and times, sequences, collections, and mapping types
* The collections module
* Enumerations

## Everything is an object

Before we delve into the specifics, we want you to be very clear about objects in Python, so let's talk a little bit more about them. As we already said, everything in Python is an object. But what really happens when you type an instruction like **age = 42** in a Python module?

So, what happens is that an object is created. It gets an id, the type is set to int (integer number), and the value to 42. A name, age, is placed in the global namespace, pointing to that object. Therefore, whenever we are in the global namespace, after the execution of that line, we can retrieve that object by simply accessing it through its name: **age**.

## Mutable or immutable? That is the question

The first fundamental distinction that Python makes on data is about whether or not the value of an object can change. If the value can change, the object is called **mutable**, whereas if the value cannot change, the object is called **immutable**. It is very important that you understand the distinction between mutable and immutable because it affects the code you write; take this example:

```bash
    >>> age = 42
    >>> age
    42
    >>> age = 43 #A
    >>> age
    43
```

**Important:** each object has an **id**, a **type** (e.g int) and a **value** (e.g. 42). The **name** (e.g. a) is a global namespace pointing to that object.

In the preceding code, on line #A, have we changed the value of age? Well, no. But now it's 43 (we hear what you are saying...). Yes, it's 43, but 42 was an integer number, of the type int, which is **immutable**. 

So, what happened is really that on the first line, age is a **name** that is set to point to an **int** object, whose **value** is 42. When we type age = 43, what happens is that another object is created, of the type int and value 43 (also, the id will be different), and the name age is set to point to it. 

So, in fact, we did not change that 42 to 43—we actually just pointed age to a different location, which is the new int object whose value is 43. Let's see the same code also printing the IDs:

```bash
    >>> age = 42
    >>> id(age)
    4377553168
    >>> age = 43
    >>> id(age)
    4377553200
```

Notice that we print the IDs by calling the built-in ``id()`` function. As you can see, they are different, as expected. Bear in mind that age points to one object at a time: 42 first, then 43—never together.

Now, let's see the same example using a **mutable** object. For this example, let's just use a ``Person`` object, that has a property age:

```bash
    >>> class Person:
    ... def __init__(self, age):
    ... self.age = age
    ...
    >>> fab = Person(age=42)
    >>> fab.age
    42
    >>> id(fab)
    4380878496
    >>> id(fab.age)
    4377553168
    >>> fab.age = 25 # I wish!
    >>> id(fab) # will be the same
    4380878496
    >>> id(fab.age) # will be different
    4377552624
```

In this case, we set up an object fab whose type is Person (a custom class). On creation, the object is given the age of 42. We then print it, along with the object ID, and the ID of age as well. Notice that, even after we change age to be 25, the ID of fab stays the same (while the ID of age has changed, of course). Custom objects in Python are mutable (unless you code them not to be). Keep this concept in mind, as it's very important.

## Numbers

Let's start by exploring Python's built-in data types for numbers. Python was designed by a man with a master's degree in mathematics and computer science, so it's only logical that it has amazing support for numbers.

> Numbers are **immutable** objects.

## Integers

Python integers have an unlimited range, subject only to the available virtual memory. This means that it doesn't really matter how big a number you want to store is—as long as it can fit in your computer's memory, Python will take care of it.

Integer numbers can be positive, negative, or 0 (zero). They support all the basic mathematical operations, as shown in the following example:

```bash
    >>> a = 14
    >>> b = 3
    >>> a + b # addition
    17
    >>> a - b # subtraction
    11
    >>> a * b # multiplication
    42
    >>> a / b # true division
    4.666666666666667
    >>> a // b # integer division
    4
    >>> a % b # modulo operation (reminder of division)
    2
    >>> a ** b # power operation
    2744
```

The preceding code should be easy to understand. Just notice one important thing: Python has two division operators, one performs the so-called **true division** (/), which returns the quotient of the operands, and another one, the so-called **integer division** (//), which returns the floored quotient of the operands.

Let's see how division behaves differently when we introduce negative numbers:

```bash
    >>> 7 / 4 # true division
    1.75
    >>> 7 // 4 # integer division, truncation returns 1
    1
    >>> -7 / 4 # true division again, result is opposite of previous
    -1.75
    >>> -7 // 4 # integer div., result not the opposite of previous
    -2
```

This is an interesting example. If you were expecting a -1 on the last line, don't feel bad, it's just the way Python works. Integer division in Python is always rounded toward minus infinity. If, instead of flooring, you want to truncate a number to an integer, you can use the built-in ``int()`` function, as shown in the following example:

```bash
    >>> int(1.75)
    1
    >>> int(-1.75)
    -1
```

It's worth noting that the power operator, ``**``, also has a built-in function counterpart, ``pow()``, shown in the example below:

```bash
    >>> pow(10, 3)
    1000.0 # result is float
    >>> 10 ** 3
    1000 # result is int
    >>> pow(10, -3)
    0.001
    >>> 10 ** -3
    0.001
```

One nice feature introduced in Python 3.6 is the ability to add underscores within number literals (between digits or base specifiers, but not leading or trailing). The purpose is to help make some numbers more readable, such as **1_000_000_000**:

```bash
    >>> n = 1_024
    >>> n
    1024
    >>> hex_n = 0x_4_0_0 # 0x400 == 1024
    >>> hex_n
    1024
```

## Booleans

Boolean algebra is that subset of algebra in which the values of the variables are the truth values, true and false. In Python, True and False are two keywords that are used to represent truth values. Booleans are a subclass of integers, so **True** and **False** behave respectively like 1 and 0. The equivalent of the int class for Booleans is the bool class,
which returns either True or False. Every built-in Python object has a value in the Boolean context, which means they basically evaluate to either True or False when fed to the bool function.

Boolean values can be combined in Boolean expressions using the logical operators and, or, and not.

```bash
    >>> int(True) # True behaves like 1
    1
    >>> int(False) # False behaves like 0
    0
    >>> bool(1) # 1 evaluates to True in a Boolean context
    True
    >>> bool(-42) # and so does every non-zero number
    True
    >>> bool(0) # 0 evaluates to False
    False
    >>> # quick peek at the operators (and, or, not)
    >>> not True
    False
    >>> not False
    True
    >>> True and True
    True
    >>> False or True
    True
```

You can see that True and False are subclasses of integers when you try to add them. Python upcasts them to integers and performs the addition:

```bash
    >>> 1 + True
    2
    >>> False + 42
    42
    >>> 7 - True
    6
```

## Real numbers

Real numbers, or **floating point numbers**, are represented in Python according to the **IEEE 754** double-precision binary floating point format, which is stored in 64 bits of information divided into three sections: sign, exponent, and mantissa.

Several programming languages give coders two different formats: single and double precision. The former takes up 32 bits of memory, the latter 64. Python supports only the double format. Let's see a simple example:

```bash
    >>> pi = 3.1415926536 # how many digits of PI can you remember?
    >>> radius = 4.5
    >>> area = pi * (radius ** 2)
    >>> area
    63.617251235400005
```

**Important:** double precision numbers suffer from approximation issues even when it comes to simple numbers like 0.1 or 0.3. Why is this important? It can be a big problem if you are handling prices, or financial calculations, or any kind of data that need not to be approximated. Don't worry, Python gives you the Decimal type, which doesn't suffer from these issues; 

## Fractions and decimals

Let's finish the tour of the number department with a look at fractions and decimals. Fractions hold a rational numerator and denominator in their lowest forms. Let's see a quick example:

```bash
    >>> from fractions import Fraction
    >>> Fraction(10, 6) # mad hatter?
    Fraction(5, 3) # notice it's been simplified
    >>> Fraction(1, 3) + Fraction(2, 3) # 1/3 + 2/3 == 3/3 == 1/1
    Fraction(1, 1)
    >>> f = Fraction(10, 6)
    >>> f.numerator
    5
    >>> f.denominator
    3
    >>> f.as_integer_ratio()
    (5, 3)
```

The **as_integer_ratio()** method has also been added to integers and Booleans. This is helpful, as it allows you to use it without needing to worry about what type of number is being worked with.

Although Fraction objects can be very useful at times, it's not that common to spot them in commercial software. Instead, it is much more common to see decimal numbers being used in all those contexts where precision is everything; for example,
in scientific and financial calculations.

**Note:** It's important to remember that arbitrary precision decimal numbers come at a price in terms of performance, of course. The amount of data to be stored for each number is greater than it is for Fractions or floats. The way they are handled also requires the Python interpreter to work harder behind the scenes. Another interesting thing to note is that you can get and set the precision by accessing ``decimal.getcontext().prec``.

## Let's see a quick example with decimal numbers

```bash
    >>> from decimal import Decimal as D # rename for brevity
    >>> D(3.14) # pi, from float, so approximation issues
    Decimal('3.140000000000000124344978758017532527446746826171875')
    >>> D('3.14') # pi, from a string, so no approximation issues
    Decimal('3.14')
    >>> D(0.1) * D(3) - D(0.3) # from float, we still have the issue
    Decimal('2.775557561565156540423631668E-17')
    >>> D('0.1') * D(3) - D('0.3') # from string, all perfect
    Decimal('0.0')
    >>> D('1.4').as_integer_ratio() # 7/5 = 1.4 (isn't this cool?!)
    (7, 5)
```

Notice that when we construct a Decimal number from a float, it takes on all the approximation issues a float may come with. On the other hand, when we create a Decimal from an integer or a string representation of a number, then the Decimal will
have no approximation issues, and therefore no quirky behavior. When it comes to currency or situations in which precision is of utmost importance, use decimals. 

This concludes our introduction to built-in numeric types. Let's now look at sequences.

## Immutable sequences

Let's start with immutable sequences: strings, tuples, and bytes.

## Strings and bytes

Textual data in Python is handled with ``str`` objects, more commonly known as **strings**. They are immutable sequences of **Unicode code points**. Unicode code points can represent a character, but can also have other meanings, such as when formatting, for example. Python, unlike other languages, doesn't have a **char** type, so a single character is rendered simply by a string of length 1.

Unicode is an excellent way to handle data, and should be used for the internals of any application. When it comes to storing textual data though, or sending it on the network, you will likely want to encode it, using an appropriate encoding for the medium you are using. The result of an encoding produces a bytes object, whose syntax and behavior is similar to that of strings. String literals are written in Python using single, double, or triple quotes (both single or double). If built with triple quotes, a string can span multiple lines. An example will clarify this:

```bash
    >>> # 4 ways to make a string
    >>> str1 = 'This is a string. We built it with single quotes.'
    >>> str2 = "This is also a string, but built with double quotes."
    >>> str3 = '''This is built using triple quotes,
    ... so it can span multiple lines.'''
    >>> str4 = """This too
    ... is a multiline one
    ... built with triple double-quotes."""
    >>> str4 #A
    'This too\nis a multiline one\nbuilt with triple double-quotes.'
    >>> print(str4) #B
    This too
    is a multiline one
    built with triple double-quotes.
```

Strings, like any sequence, have a length. You can get this by calling the ``len()`` function:

```bash
    >>> str1 = 'This is a string. We built it with single quotes.'
    >>> len(str1)
    49
```

Python 3.9 has introduced two new methods that deal with the prefixes and suffixes of strings. Here's an example that explains the way they work:

```bash
    >>> s = 'Hello There'
    >>> s.removeprefix('Hell')
    'o There'
    >>> s.removesuffix('here')
    'Hello T'
    >>> s.removeprefix('Ooops')
    'Hello There'
```

The nice thing about them is shown by the last instruction: when we attempt to remove a prefix or suffix which is not there, the method simply returns a copy of the original string. This means that these methods, behind the scenes, are checking if
the prefix or suffix matches the argument of the call, and when that's the case, they remove it.

## Encoding and decoding strings

Using the encode/decode methods, we can encode Unicode strings and decode bytes objects. UTF-8 is a variable-length character encoding, capable of encoding all possible Unicode code points. It is the most widely used encoding for the web.
Notice also that by adding the literal b in front of a string declaration, we're creating a bytes object:

```bash
    >>> s = "This is üŋíc0de" # unicode string: code points
    >>> type(s)
    <class 'str'>
    >>> encoded_s = s.encode('utf-8') # utf-8 encoded version of s
    >>> encoded_s
    b'This is \xc3\xbc\xc5\x8b\xc3\xadc0de' # result: bytes object
    >>> type(encoded_s) # another way to verify it
    <class 'bytes'>
    >>> encoded_s.decode('utf-8') # let's revert to the original
    'This is üŋíc0de'
    >>> bytes_obj = b"A bytes object" # a bytes object
    >>> type(bytes_obj)
    <class 'bytes'>
```

## Indexing and slicing strings

When manipulating sequences, it's very common to access them at one precise position (indexing), or to get a sub-sequence out of them (slicing). When dealing with immutable sequences, both operations are read-only. While indexing comes in one form—zero-based access to any position within the sequence—slicing comes in different forms. When you get a slice of a sequence, you can specify the start and stop positions, along with the step. They are separated with a colon ( : ) like this: my_sequence[start:stop:step]. All the arguments are optional; start is inclusive, and stop is exclusive. It's probably better to see an example, rather than try to explain them any further with words:

```python
    s = "The trouble is you think you have time."

    print(s[0])  # indexing at position 0, which is the first char
    print(s[5])  # indexing at position 5, which is the sixth char
    print(s[:4])  # slicing, we specify only the stop position
    print(s[4:])  # slicing, we specify only the start position
    print(s[2:14])  # slicing, both start and stop positions
    print(s[2:14:3])  # slicing, start, stop and step (every 3 chars)
    print(s[:])  # quick way of making a copy
```

Returns:

> T     
> r     
> The       
> trouble is you think you have time.       
> e trouble is      
> erb       
> The trouble is you think you have time.

The last line is quite interesting. If you don't specify any of the parameters, Python will fill in the defaults for you. In this case, start will be the start of the string, stop will be the end of the string, and step will be the default: 1. This is an easy and quick way of obtaining a copy of the string s (the same value, but a different object). Can you think of a way to get the reversed copy of a string using slicing (don't look it up—find it for yourself)?

```bash
    print(s[::-1])
```

Returns:

> .emit evah uoy kniht uoy si elbuort ehT

## String formatting

One of the features strings have is the ability to be used as a template. There are several different ways of formatting a string, and for the full list of possibilities, we encourage you to look up the documentation. Here are some common examples:

```python
    greet_old = "Hello %s!"
    print(greet_old % "Alan")

    greet_positional = "Hello {}!"
    print(greet_positional.format("Alan"))

    greet_positional = "Hello {} {}!"
    greet_positional.format("Alan", "Robson")

    greet_positional_idx = "This is {0}! {1} loves {0}!"
    print(greet_positional_idx.format("Python", "James"))

    print(greet_positional_idx.format("Coffee", "Al"))

    keyword = "Hello, my name is {name} {last_name}"
    print(keyword.format(name="Alan", last_name="Robson"))
```

One last feature we want to show you was added to Python in version 3.6, and it's called **formatted string literals**. This feature is quite cool (and it is faster than using the ``format()`` method): strings are prefixed with f, and contain replacement fields surrounded by curly braces.

Replacement fields are expressions evaluated at runtime, and then formatted using
the format protocol:

```python
    name = "Alan"
    age = 71

    message = f"Hello! My name is {name} and I'm {age}"

    print(message)

    from math import pi

    message2 = f"No arguing with {pi}, it's irrational..."

    print(message2)
```

Returns:

> Hello! My name is Alan and I'm 71     
> No arguing with 3.141592653589793, it's irrational...

An interesting addition to f-strings, which was introduced in Python 3.8, is the ability to add an **equals** sign specifier within the f-string clause; this causes the expression to expand to the text of the expression, an equals sign, then the representation of the evaluated expression. This is great for self-documenting and debugging purposes. 

```python
    user = "alan"
    password = "super-secret"

    print(f"Log in with: {user} and {password}")

    print(f"Log in with: {user=} and {password=}")
```

Returns:

> Log in with: alan and super-secret        
> Log in with: user='alan' and password='super-secret'

Some more examples of f-strings.

```python
    name = "Alan"
    age = 71

    print(f"Hello, My name is {name} and I'm {age} years old.")
```

> Hello, My name is Alan and I'm 71 years old.

```python
    # Prints today's date with help
    # of datetime library
    import datetime

    today = datetime.datetime.today()
    print(f"{today:%B %d, %Y}")
```

> October 10, 2023

## Tuples

The last immutable sequence type we are going to look at here is the tuple. A tuple is a sequence of arbitrary Python objects. In a tuple declaration, items are separated by commas. Tuples are used everywhere in Python. They allow for patterns that are quite hard to reproduce in other languages. Sometimes tuples are used implicitly; for example, to set up multiple variables on one line, or to allow a function to return multiple objects (in several languages, it is common for a function to return only one object), and in the Python console, tuples can be used implicitly to print multiple elements with one single instruction.

```python
    t = ()  # empty tuple
    print(type(t))

    one_element_tuple = (42,)  # you need the comma!
    three_elements_tuple = (1, 3, 5)  # braces are optional here

    a, b, c = 1, 2, 3  # tuple for multiple assignment
    print(a, b, c)  # implicit tuple to print with one instruction

    print(3 in three_elements_tuple)  # membership test
```

Returns:

> <class 'tuple'>       
> 1 2 3     
> True

Notice that the membership operator in can also be used with lists, strings, dictionaries, and, in general, with collection and sequence objects.

**Note:** Notice that to create a tuple with one item, we need to put a comma after the item. The reason is that without the comma that item is wrapped in braces on its own, in what can be considered a redundant mathematical expression. Notice also that on assignment, braces are optional, so my_tuple = 1, 2, 3 is the same as my_tuple = (1, 2, 3).

One thing that tuple assignment allows us to do is *one-line swaps*, with no need for a third temporary variable. Let's first see the traditional way of doing it:

```python
    a, b = 1, 2
    c = a  # we need three lines and a temporary var c
    a = b
    b = c
    a, b  # a and b have been swapped
    print(a, b)
    print(c)
    
    a, b = 0, 1
    a, b = b, a  # this is the Pythonic way to do it
    print(a, b)
```

Returns:

> 2 1       
> 1     
> 1 0

Take a look at the line that shows you the Pythonic way of swapping two values. A Python program is typically one-fifth to one-third the size of equivalent Java or C++ code, and features like one-line swaps contribute to this. Python is elegant, where elegance in this context also means economy.

Because they are immutable, tuples can be used as keys for dictionaries (we'll see this shortly). To us, tuples are Python's built-in data that most closely represent a mathematical vector. This doesn't mean that this was the reason for which they were created, though. Tuples usually contain a heterogeneous sequence of elements while, on the other hand, lists are, most of the time, homogeneous. Moreover, tuples are normally accessed via unpacking or indexing, while lists are usually iterated over.

## Mutable sequences

**Mutable** sequences differ from their **immutable** counterparts in that they can be changed after creation. There are two mutable sequence types in Python: lists and byte arrays.

## Lists

Python lists are very similar to tuples, but they don't have the restrictions of immutability. Lists are commonly used for storing collections of homogeneous objects, but there is nothing preventing you from storing heterogeneous collections
as well. Lists can be created in many different ways.

```python
        []  # empty list
        print([])

        x = list()  # same as []
        print(x)

        a = [1, 2, 3]  # as with tuples, items are comma separated
        print(a)

        b = [x + 5 for x in [2, 3, 4]]  # Python is magic
        print(b)

        c = list((1, 3, 5, 7, 9))  # list from a tuple
        print(c)

        d = list("hello")  # list from a string
        print(d)
```

Returns:

> []        
> []        
> [1, 2, 3]     
> [7, 8, 9]     
> [1, 3, 5, 7, 9]       
> ['h', 'e', 'l', 'l', 'o']

In the previous example, we showed you how to create a list using various techniques. We would like you to take a good look at the line with the comment Python is magic, which we don't expect you to fully understand at this point—especially if you are unfamiliar with Python. That is called a list comprehension: a very powerful functional feature of Python.

Creating lists is good, but the real fun begins when we use them, so let's see the main methods they gift us with:

```python
    a = [1, 2, 1, 3]
    a.append(13)  # we can append anything at the end
    print(a)

    print(a.count(1))  # how many `1s` are there in the list?

    a.extend([5, 7])  # extend the list by another (or sequence)
    print(a)

    print(a.index(13))  # position of `13` in the list (0-based indexing)

    a.insert(0, 17)  # insert `17` at position 0
    print(a)

    print(a.pop())  # pop (remove and return) last element

    a.pop(3)  # pop element at position 3
    print(a)

    a.remove(17)  # remove `17` from the list
    print(a)

    a.reverse()  # reverse the order of the elements in the list
    print(a)

    a.sort()  # sort the list
    print(a)

    for item in a:
        print(item)

    a.clear()  # remove all elements from the list
    print(a)
```

Returns:

> [1, 2, 1, 3, 13]      
> 2     
> [1, 2, 1, 3, 13, 5, 7]        
> 4     
> [17, 1, 2, 1, 3, 13, 5, 7]        
> 7     
> [17, 1, 2, 3, 13, 5]      
> [1, 2, 3, 13, 5]      
> [5, 13, 3, 2, 1]      
> [1, 2, 3, 5, 13]      
> 1     
> 2     
> 3     
> 5     
> 13        
> []

**Note:** this is how you iterate through a list.

```python
    a = [1, 2, 3, 5, 13]

    for item in a:
        print(item)
```

The preceding code gives you a roundup of a list's main methods. There is also the method ``extend()``. You can extend lists using any sequence type:

```python
    a = list("hello")  # makes a list from a string
    print(a)

    a.append(100)  # append 100, heterogeneous type
    print(a)

    a.extend((1, 2, 3))  # extend using tuple
    print(a)

    a.extend("...")  # extend using string
    print(a)
```

Returns:

> ['h', 'e', 'l', 'l', 'o']     
> ['h', 'e', 'l', 'l', 'o', 100]        
> ['h', 'e', 'l', 'l', 'o', 100, 1, 2, 3]       
> ['h', 'e', 'l', 'l', 'o', 100, 1, 2, 3, '.', '.', '.']
