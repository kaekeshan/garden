---
title: Exception Handling in Python
draft: false
tags: [dev,python]
date: 2025-12-06
---

## Why ?

An error in python, can be syntactical error or an exception. 
- exception : a event causing  disruption in the normal flow of execution.  
- exception handling : process of responding to the occurrence of exceptions, during computation, of exceptional conditions requiring special processing, often changing the flow of program execution.

## Process ?

Error → user finds it OR python finds it → handling the exceptions

Find error → Take caution **(try)** → fix error **(catch)**

### Terms

1. **Try**    : code under check
2. **Except** : handle the exception if try has any
3. **Else**    : run if there is no exceptions in try block
4. **Finally** : runs any way

![](../assets/exception/Untitled.png)

## Codes

⇒ Syntax Error : occur when the parser run into  incorrect statement, format wise

⇒ Exception    : occur syntactically correct code run into error, for default cases, python highlights the type of exception the code is having like 'ZeroDivisionError'

 python has built in exceptions and the possibility to create user defined exceptions.

we can use raise to throw an exception if a condition occurs

example 

```python
x = 10
if x < 5:
	raise Exception('x should not exceed five. The value of x was : {}'.format(x))

```

---

### AssertionError Exception

Instead of waiting for a program to crash midway, you can also start by making an assertion in python.

we assert that a certain condition is met, if the condition is true, then the program continuous to execute, if false , then an assertion error exception is thrown.

assert: 
{ test if the condition is true

```python
import sys
assert ('linux' in sys.platform), "This code runs on linux only. "
```

 If assertion error occurs, the program comes to a halt.

using try and except to catch and handle exception exception to circumvent the resultant halt on the occurrence of an exception.

```python
def linux_interaction():
	assert ('linux' in sys.platform), "Function can only run on linux system only. "
  print("doing something..")

#handling the exception 
try:
	linux_interaction()
except:
  print("Linux function was not executed")
```

// to catch the accurate error message that was thrown for an exception

```python
try:
	linux_interaction()
except AssertionError as error:
  print(error) # actual error
  print("The linux_interaction() function was not executed")
```

// here the error will contain the error we specified with assert : "Function can only run on Linux system only"

another example :

```python
try:
	with open('file.log') as file:
		read_data = file.read()
except FileNotFoundError as fnf_error
	 print(fnf_error)
```

// exception error becomes invisible if bare exception statements are used.

// we can anticipate multiple exceptions 

// in try block, the execution of statements inside try block stops immediately if it runs into an exception, the remaining statements will not be executed.

### Else

 using else statements, we can instruct a program to execute a certain block of statements in the absence of exceptions.

 we can also try to run code inside the else clause and catch possible exceptions there as well.

### Finally

 the statement that runs anyways.

![](../assets/exception/Untitled%201.png)

---

## Creating custom exceptions classes

 since all type of exceptions like ValueError etc. are sub classes of main class Exceptions, we have to implement inheritance

```python
class CustomException(Exception):
	def __init__(self,msg):
		super().__init__(msg)
```

// now we can raise 'CustomException' anywhere we like

example:

![](../assets/exception/Untitled%202.png)
