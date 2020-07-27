## 3. Control flow
### 3.1 Conditional execution
The general form of a conditional expression is:  
```python
if expression1:
    statements1
elif expression2:
    statements2
elif expression3:
    statements3
else:
    statements4
```

The _elif_ and _else_ clauses are _optional_.  
Use the _pass_ statement if not statement exists for a particular clause.  

_Example_: print the maximum among 3 integers.  
```python
a = int(input("Enter the first integer: "))
b = int(input("Enter the second integer: "))
c = int(input("Enter the third integer: "))
if a > b:
    if a > c:
        print("The maximum is", a)
    else:
        print("The maximum is", c)
else:
    if b > c:
        print("The maximum is", b)
    else:
        print("The maximum is", c)
```

Here, the _input_ function returns a string containing the input entered by the user.  
The _int_ function converts the string returned by the _input_ function to an integer or _raises_ a _ValueError_ exception if the conversion could not be done.

### 3.2 Loops and iteration
#### 3.2.1 While loops
_While_ loops are used to execute statements until the associated expression is evaluated to _False_.  
```python
while expression:
    statements
```
The _break_ statement can be used to exit from the loop.  
The _continue_ statement can be used to immediately continue the execution at the start of the loop.

_Example_: the guessing game.  
The program selects a random number between 1 and 100. The player task is to guess that number.  
The player has only 7 attempts to guess the number.  
At each unsuccessful attempt, the player receives an indication whether the guessed number is either smaller or bigger than the correct number.  
Open a file in your favorite text editor, add the code below and save it as guess.py

```python
import random

# Select a random number between 1 and 100. 
number = random.randint(1, 100)

number_guessed = False
guesses_taken = 0

while guesses_taken < 7:
    guessed_number = int(input("Guess the number: "))
    guesses_taken += 1
    
    if guessed_number < number:
        print("Your number is too small")
    elif guessed_number > number:
        print("Your number is too big")
    else:
        number_guessed = True
        break
        
if number_guessed:
    print("Congratulations. You have guessed the correct number,", number)
else:
    print("You failed to guess the number", number) 
```

From the terminal (or _Command Prompt_ on _Windows_), execute:

```python
python guess.py
```

Observe what is happening.  

#### 3.2.2 The _for_ statement
The _for_ statement is used to _iterate_ over a _sequence_ or an _iterable_ object.  

_Example_: iterate over each character of a string and display it.  

```python
>>> for i in "Hello Python":
>>>     print(i, end=' ')
H e l l o   P y t h o n
```

If you want to iterate of sequences of integers, you can use _range_.  
The following example displays the integers starting from 1 until 9.  
```python
>>> for i in range(1, 10):
>>>     print(i, end=' ')
>>> 1 2 3 4 5 6 7 8 9
```
_range_ can receive an _optional_ third parameter which represents an iteration _step_. If _step_ is not specified, the value _1_ is used.  
```python
>>> for i in range(1, 10 ,2):
>>>     print(i, end=' ')
>>> 1 3 5 7 9
```
We can use a negative _step_ parameter to iterate in reverse.  
```python
>>> for i in range(9, 0, -1):
>>>     print(i, end=' ')
>>> 9 8 7 6 5 4 3 2 1
```
