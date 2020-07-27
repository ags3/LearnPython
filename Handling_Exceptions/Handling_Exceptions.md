## 6. Raising and handling exceptions
We have seen that, in Python, exceptions are used to signal errors.  
For example:

```python
>>> 1/0
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ZeroDivisionError: division by zero
>>>
```

As we see, not surprisingly, an exception is _raised_ when we try to divide a number to zero.  
In fact, we can rise exceptions on our own. Here is an example:  

```python
>>> def divide(a, b):
...     if b == 0:
...         raise ZeroDivisionError("Cannot divide by zero !")
...     return a/b
...
>>> divide(4, 2)
2.0
>>> divide(4, 0)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 3, in divide
ZeroDivisionError: Cannot divide by zero !
>>>
```

We can deal with exceptions by _handling it_. For this, we use a _try except_ block.  

```python
>>> try:
...     1/0
... except:
...     print("Division by zero attempt!")
...
Division by zero attempt!
```

Here, we capture all the possible exceptions. We can capture a specific exception type as follows:  
 
```python
>>> try:
...     1/0
... except ZeroDivisionError as e:
...     print("Exception caught: %s" % e)
...
Exception caught: division by zero
```

We can execute a block if an exception was not raised:

```python
>>> try:
...     4/2
... except:
...     print("What ?!?")
... else:
...     print("No exception was caught")
...
2.0
No exception was caught
>>>
```

We can also _re-raise_ an exception:

```python
>>> try:
...     1/0
... except:
...     print("An exception appeared")
...     raise
...
An exception appeared
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
ZeroDivisionError: division by zero
>>>
```

Sometimes, we want to execute a block a code regardless if an exception is raised or not.  
As an example, we want to close a successfully opened file, regardless if an exception was raised or not.  

```python
>>> try:
...    1/0
... except:
...    print("An exception was caught")
... finally:
...    print("But the finally clause is still executed")
...
An exception was caught
But the finally clause is still executed
>>>
```

```python
>>> try:
...     4/2
... except:
...     print("What ?!?")
... finally:
...     print("The finally clause is still executed")
...
2.0
The finally clause is still executed
>>>
```
