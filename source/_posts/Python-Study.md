---
title: Python Study
date: 2016-12-19 19:46:52
tags:
---
`19 Dec 2016`

[TOC]

#BASIC SYNTAX
<!--more-->
## Lines and Indentations
Python provides no braces to indicate blocks of code for class and function definitions or flow control. Blocks of code are denoted by line indentation, which is rigidly enforced.
Thus, in Python all the continuous lines indented with same number of spaces would form a block.

## Quotation
Python accepts single ('), double (") and triple (''' or """) quotes to denote string literals, as long as the same type of quote starts and ends the string.

## Comments
A hash sign (#) that is not inside a string literal begins a comment. 

## Multiple Statement Groups as Suites
A group of individual statements, which make a single code block are called suites in Python. Compound or complex statements, such as if, while, def, and class require a header line and a suite. 
Header lines begin the statement (with the keyword) and terminate with a colon ( : ) and are followed by one or more lines which make up the suite. 

```python
if expression :    
    suite 
elif expression :    
    suite 
else : 
    suite
```

# BASIC OPERATORS

- ** :Exponent Performs 
- and: Logical AND
- or: Logical OR
- not: Logical NOT
- is/is not: Evaluates to true if the variables on either side of the operator point to the same object and false otherwise.
- in/not in: Evaluates to true if it finds a variable in the specified sequence and false otherwise.

# VARIABLE
use ‘del’ key word to delete the reference to value of any variable.
 
## Standard Type
Python has five standard data types:  Numbers  String  List  Tuple  Dictionary.

## Numbers
Python supports four different numerical types: 
 int (signed integers)
 long (long integers, they can also be represented in octal and hexadecimal)
 float (floating point real values)
 complex (complex numbers)

## Python Strings
Strings in Python are identified as a contiguous set of characters represented in the quotation marks. Python allows for either pairs of single or double quotes. Subsets of strings can be taken using the slice operator ([ ] and [:] ) with indexes starting at 0 in the beginning of the string and working their way from -1 at the end.

## Python Lists
Lists are the most versatile of Python's compound data types. A list contains items separated by commas and enclosed within square brackets ([]). To some extent, lists are similar to arrays in C. One difference between them is that all the items belonging to a list can be of different data type. 

The values stored in a list can be accessed using the slice operator ([ ] and [:]) with indexes starting at 0 in the beginning of the list and working their way to end -1. The plus (+) sign is the list concatenation operator, and the asterisk (*) is the repetition operator. 

## Python Tuples
A tuple is another sequence data type that is similar to the list. A tuple consists of a number of values separated by commas. Unlike lists, however, tuples are enclosed within parentheses. 

The main differences between lists and tuples are: Lists are enclosed in brackets ( [ ] ) and their elements and size can be changed, while tuples are enclosed in parentheses ( ( ) ) and cannot be updated. Tuples can be thought of as read- only lists.
```python
tuple = ( 'abcd', 786 , 2.23, 'john', 70.2 ) 
list = [ 'abcd', 786 , 2.23, 'john', 70.2 ] 
tuple[2] = 1000 # Invalid syntax with tuple 
list[2] = 1000 # Valid syntax with list
```
## Python Dictionary
Python's dictionaries are kind of hash table type. They work like associative arrays or hashes found in Perl and consist of key-value pairs. A dictionary key can be almost any Python type, but are usually numbers or strings. Values, on the other hand, can be any arbitrary Python object. 

Dictionaries are enclosed by curly braces ({ }) and values can be assigned and accessed using square braces ([ ])
```python
dict = {} 
dict['one'] = "This is one" 
dict[2] = "This is two" 
tinydict = {'name': 'john','code':6734, 'dept': 'sales'} 
print dict['one'] # Prints value for 'one' key
print dict[2]             # Prints value for 2 key
print tinydict              # Prints complete dictionary
print tinydict.keys()  # Prints all the keys
print tinydict.values()    # Prints all the values
```
## Data Type Conversion
int(x [,base]) Converts x to an integer. base specifies the base if x is a string. 
long(x [,base] ) Converts x to a long integer. base specifies the base if x is a string. 
float(x) Converts x to a floating-point number.
str(x) Converts object x to a string representation.
list(s) Converts s to a list. 
set(s) Converts s to a set. 
dict(d) Creates a dictionary. d must be a sequence of (key,value) tuples.
chr(x) Converts an integer to a character. 
unichr(x) Converts an integer to a Unicode character. 
ord(x) Converts a single character to its integer value. 
hex(x) Converts an integer to a hexadecimal string. 
oct(x) Converts an integer to an octal string.

# DECISION MAKING

## If Statement
```python
if expression1:    
    statement(s) 
elif expression2:    
    statement(s) 
elif expression3:    
    statement(s) 
else:    
    statement(s)
```
# LOOPS

## While Loop
```python
while expression:
    statement(s)
#—————————
count = 0 
while (count < 9):
    print 'The count is:', count
    count = count + 1
```

## For Loop
```python
for iterating_var in sequence:
    statements(s)
#———————————
for letter in 'Python': # First Example
     print 'Current Letter :', letter

fruits = ['banana', 'apple',  'mango'] 
for index in range(len(fruits)): 
    print 'Current fruit :', fruits[index]
```
## Pass Statement
It is used when a statement is required syntactically but you do not want any command or code to execute.
The pass statement is a null operation; nothing happens when it executes. The pass is also useful in places where your code will eventually go, but has not been written yet 

# NUMBERS

## Mathematical Functions
abs(x) The absolute value of x: the (positive) distance between x and zero. 
ceil(x) The ceiling of x: the smallest integer not less than x 
cmp(x, y) -1 if x < y, 0 if x == y, or 1 if x > y 
exp(x) The exponential of x: ex 
fabs(x) The absolute value of x. 
floor(x) The floor of x: the largest integer not greater than x 
log(x) The natural logarithm of x, for x> 0 log10(x) The base-10 logarithm of x for x> 0. 
max(x1, x2,...) The largest of its arguments: the value closest to positive infinity 
min(x1, x2,...) The smallest of its arguments: the value closest to negative infinity
modf(x) The fractional and integer parts of x in a two-item tuple. Both parts have the same sign as x. The integer part is returned as a float. 
pow(x, y) The value of x**y.
round(x [,n]) x rounded to n digits from the decimal point. Python rounds away from zero as a tie-breaker: round(0.5) is 1.0 and round(- 0.5) is -1.0. 
sqrt(x) The square root of x for x > 0

## Mathematical Constants   
pi The mathematical constant pi. 
e The mathematical constant e.

#  STRINGS

## String Formatting Operator
One of Python's coolest features is the string format operator %. This operator is unique to strings and makes up for the pack of having functions from C's printf() family.
```python
print "My name is %s and weight is %d kg!" % ('Zara', 21)
```
## Triple Quotes
Python's triple quotes comes to the rescue by allowing strings to span multiple lines, including verbatim NEWLINEs, TABs, and any other special characters.

## Built-in String Methods
- capitalize() Capitalizes first letter of string.
- count(str, beg= 0,end=len(string)) Counts how many times str occurs in string or in a substring of string if starting index beg and ending index end are given.
- find(str, beg=0 end=len(string)) Determine if str occurs in string or in a substring of string if starting index beg and ending index end are given returns index if found and -1 otherwise.
- index(str, beg=0, end=len(string)) Same as find(), but raises an exception if str not found.
- isdigit() Returns true if string contains only digits and false otherwise.
- len(string) Returns the length of the string.
- split(str="", num=string.count(str)) Splits string according to delimiter str (space if not provided) and returns list of substrings; split into at most num substrings if given.

# LISTS

## Built-in List Functions and Methods
- cmp(list1, list2) Compares elements of both lists.
- len(list) Gives the total length of the list.
- max(list) Returns item from the list with max value.
- min(list) Returns item from the list with min value.
- list(seq) Converts a tuple into list.

- list.append(obj) Appends object obj to list
- list.count(obj) Returns count of how many times obj occurs in list
- list.extend(seq) Appends the contents of seq to list
- list.index(obj) Returns the lowest index in list that obj appears
- list.insert(index, obj) Inserts object obj into list at offset index 
- list.pop(obj=list[-1]) Removes and returns last object or obj from list 
- list.remove(obj) Removes object obj from list 
- list.reverse() Reverses objects of list in place 
- list.sort([func]) Sorts objects of list, use compare func if given

# FUNCTIONS

## Defining a Function
```python
def functionname( parameters ):
    "function_docstring"
    function_suite
    return [expression]
```
## Passing by Reference Versus Passing by Value
All object parameters (arguments) in the Python language are passed by reference. It means if you change what a parameter refers to within a function, the change also reflects back in the calling function.

## Variable Length Arguments
You may need to process a function for more arguments than you specified while defining the function. These arguments are called variable-length arguments and are not named in the function definition, unlike required and default arguments.

An asterisk (*) is placed before the variable name that holds the values of all nonkeyword variable arguments. This tuple remains empty if no additional arguments are specified during the function call.

```python
def functionname([formal_args,] *var_args_tuple ):
     "function_docstring"
     function_suite
     return [expression]
#——————————————
# Function definition is here 
def printinfo( arg1, *vartuple ):
     "This prints a variable passed arguments"
      print "Output is: " print arg1
      for var in vartuple:
           print var
      return;

```

## The Anonymous Functions
These functions are called anonymous because they are not declared in the standard manner by using the def keyword. You can use the lambda keyword to create small anonymous functions.

Lambda forms can take any number of arguments but return just one value in the form of an expression. They cannot contain commands or multiple expressions.
The syntax of lambda functions contains only a single statement
`lambda [arg1 [,arg2,.....argn]]:expression`

```python
# Function definition is here 
sum = lambda arg1, arg2: arg1 + arg2; 
# Now you can call sum as a function 
print "Value of total : ", sum( 10, 20 )
```

# MODULES

A module allows you to logically organize your Python code. Grouping related code into a module makes the code easier to understand and use. A module is a Python object with arbitrarily named attributes that you can bind and reference.
Simply, a module is a file consisting of Python code. A module can define functions, classes and variables. A module can also include runnable code.

## The import Statement
You can use any Python source file as a module by executing an import statement in
some other Python source file.
`import module1[, module2[,... moduleN]`

## The from...import(*) Statement
Python's from statement lets you import specific attributes from a module into the
current namespace. 

## The PYTHONPATH Variable
The PYTHONPATH is an environment variable, consisting of a list of directories. The syntax of PYTHONPATH is the same as that of the shell variable PATH.

##  Namespaces and Scoping
Variables are names (identifiers) that map to objects. A namespace is a dictionary of variable names (keys) and their corresponding objects (values).

A Python statement can access variables in a local namespace and in the global namespace. If a local and a global variable have the same name, the local variable shadows the global variable.

Python makes educated guesses on whether variables are local or global. It assumes that any variable assigned a value in a function is local.
Therefore, in order to assign a value to a global variable within a function, you must first use the global statement.
The statement global VarName tells Python that VarName is a global variable. Python stops searching the local namespace for the variable.

# FILES I/O

## Reading Keyboard Input
- raw_input
The raw_input([prompt]) function reads one line from standard input and returns it as a string (removing the trailing newline).

- input
The input([prompt]) function is equivalent to raw_input, except that it assumes the input is a valid Python expression and returns the evaluated result to you.

## Opening and Closing Files
Python provides basic functions and methods necessary to manipulate files by default. You can do your most of the file manipulation using a file object.
- open:
`file object = open(file_name [, access_mode][, buffering])`

- close:
`fileObject.close();`

## Reading and Writing Files
- write:
`fileObject.write(string);`

- read:
`fileObject.read([count]);`

# EXCEPTIONS

# CLASSES AND OBJECTS

## Creating Classes
```python
class ClassName:
'Optional class documentation string' 
    class_suite

#-----------------
class Employee:
'Common base class for all employees'
    empCount = 0

    def __init__(self, name, salary):
        self.name = name
        self.salary = salary
        Employee.empCount += 1

   def displayCount(self):
        print "Total Employee %d" % Employee.empCount

   def displayEmployee(self):
        print "Name : ", self.name, ", Salary: ", self.salary
```

- The variable empCount is a class variable whose value is shared among all instances of a this class. This can be accessed as Employee.empCount from inside the class or outside the class.
- The first method __init__() is a special method, which is called class constructor or initialization method that Python calls when you create a new instance of this class.
- You declare other class methods like normal functions with the exception that the first argument to each method is self. Python adds the self argument to the list for you; you do not need to include it when you call the methods.

## Creating Instance Objects
To create instances of a class, you call the class using class name and pass in whatever arguments its __init__ method accepts.
```python
"This would create first object of Employee class" 
emp1 = Employee("Zara", 2000)
"This would create second object of Employee class" 
emp2 = Employee("Manni", 5000)
```

## Class Inheritance

```python
class SubClassName (ParentClass1[, ParentClass2, ...]):
     'Optional class documentation string'
     class_suite
#———————————————
class Child(Parent): # define child class
     def __init__(self):
         print "Calling child constructor"
      
      def childMethod(self):
         print 'Calling child method'
``` 

## Data Hiding
An objects attributes may or may not be visible outside the class definition. You need to name attributes with a double underscore prefix, and those attributes then are not be directly visible to outsiders.

```python
class JustCounter:
    __secretCount = 0

    def count(self):
        self.__secretCount += 1
        print self.__secretCount 
```

# REGULAR EXPRESSIONS
The module re provides full support for Perl-like regular expressions in Python. The re module raises the exception re.error if an error occurs while compiling or using a regular expression.

To avoid any confusion while dealing with regular expressions, we would use Raw Strings as r'expression'.

## The match Function
`re.match(pattern, string, flags=0)`

- pattern This is the regular expression to be matched.
- string This is the string, which would be searched to match the pattern at the beginning of string.
- flags You can specify different flags using bitwise OR (|). These are modifiers, which are listed in the table below.

There.matchfunction returns amatchobject on success,noneon failure. We usegroup(num) or groups() function of match object to get matched expression.

```python
import re 

line = "Cats are smarter than dogs" 
matchObj = re.match( r'(.*) are (.*?) .*', line, re.M|re.I) 

if matchObj:
     print "matchObj.group() : ", matchObj.group()
     print "matchObj.group(1) : ", matchObj.group(1)
     print "matchObj.group(2) : ", matchObj.group(2) 
else:
     print "No match!!"
```

## The search Function
This function searches for first occurrence of RE pattern within string with optional flags.
`re.search(pattern, string, flags=0)`

The re.search function returns a match object on success, none on failure. We use group(num) or groups() function of match object to get matched expression.

## Search and Replace
One of the most important re methods that use regular expressions is sub.
`re.sub(pattern, repl, string, max=0)`

This method replaces all occurrences of the RE pattern in string with repl, substituting all occurrences unless max provided. This method returns modified string.

# NETWORK PROGRAMMING
Python provides two levels of access to network services. At a low level, you can access the basic socket support in the underlying operating system, which allows you to implement clients and servers for both connection-oriented and connectionless protocols.

Python also has libraries that provide higher-level access to specific application-level network protocols, such as FTP, HTTP, and so on.

##  The socket Module
To create a socket, you must use the socket.socket() function available
in socket module, which has the general syntax:
`s = socket.socket (socket_family, socket_type, protocol=0)`

- socket_family: This is either AF_UNIX or AF_INET, as explained earlier.
- socket_type: This is either SOCK_STREAM or SOCK_DGRAM.
- protocol: This is usually left out, defaulting to 0.

## Server Socket Methods
- s.bind()
This method binds address (hostname, port number pair) to socket.

- s.listen()
This method sets up and start TCP listener.

- s.accept()
This passively accept TCP client connection, waiting until connection arrives (blocking).
- s.connect()
This method actively initiates TCP server connection.

## General Socket Methods
- s.recv()
This method receives TCP message

- s.send()
This method transmits TCP message

- s.recvfrom()
This method receives UDP message

- s.sendto()
This method transmits UDP message

- s.close()
This method closes socket

- socket.gethostname()
Returns the hostname.

# Comprehensions
## List Comprehension
A list comprehension consists of the following parts:

- An Input Sequence.
- A Variable representing members of the input sequence.
- An Optional Predicate expression.
- An Output Expression producing elements of the output list from members of the Input Sequence that satisfy the predicate.

```python
a_list = [1, ‘4’, 9, ‘a’, 0, 4]

squared_ints = [ e**2 for e in a_list if type(e) == types.IntType ]
```

## Set Comprehension
Set comprehensions allow sets to be constructed using the same principles as list comprehensions, the only difference is that resulting sequence is a set.

Given the list:
names = [ 'Bob', 'JOHN', 'alice', 'bob', 'ALICE', 'J', 'Bob' ]

We require the set:
{ 'Bob', 'John', 'Alice' }

The following set comprehension accomplishes this:
`{ name[0].upper() + name[1:].lower() for name in names if len(name) > 1 }`
