`!/usr/bin/env python 3`

`pipenv install`

`pipenv install <package>`


Use pipenv to install packages. It installs into your project, so anyone else can `pipenv install` and get a copy of all packages and dependencies required

`pipenv shell`


#### Data types
*str*
```
= "this is a string"
```

*int*
```
= 10
```

*float*
```
= 3.141
```

*bool*
```
= True
```

*undefined*
```
= None
```

*list* - just an array
```
= [3, 1, 4, 1]
```

*tuple* - array that cant be changed. Don't use if you only have one element
```
= (3, 1, 4, 1)
```

*set* - array that cannot have any duplicates. You **are** able to append/add here
```
= {3, 1, 4, 1}
```

*dictionary* - basically JS objects
```
dictionaryVariable = {
"key": "value",
"intkey": 10
}

print(dictionaryVariable["key"])
//=> "value"
print(dictionaryVariable["intkey"])
//=> 10

print(dictionaryVariable.get("key"))
//=> "value"

print(dictionaryVariable.get("thisisakeythatdoesntexist"))
//=> "None"


print(dictionaryVariable.get("thisisakeythatdoesntexist". "weReturnThis"))
//=> "weReturnThis"
// setting a default value to be returned (instead of "None")


dictionaryVariable["intkey"] = 11
print(dictionaryVariable["intkey"])
//=> 11

```

To convert data types, we can use `newVariable = set(oldVariable)`

Change `set()` to `bool()` or `float()` or `int()` or whatever you're trying to convert to.
**This is NON destructive**
oldVariable will remain the same data type



```
oldVariable = "3141"
print(oldVariable)
//=> "3141"

newVariable = int(oldVariable)
print(newVariable)
//=> 3141
```


#### if/elif/else

```
if True:
	print("This is going to print")
print("this will still print, but its considered OUTSIDE the if statement")


if True:
	print("If triggers here")
else:
	print("This is the else statement")


if True:
	print("If triggers here")
elif x == 10:
	print("this will trigger, only if True becomes false, AND x == 10)
else:
	pass
```

"pass" is a keyword, similar to undefined, to literally do nothing. We can stack as many elif statements as we'd like.



#### Ternary's


```
print("Ternary is true") if x == 10 else print("Ternary is false")
//This is fucking stupid ^^ :D :D :D :D :D :D :D: D: D:D :DD :D:D :D D:D :D:D:D
```

#### Conditional statements.

Obviously we have:

```
==
!=
>
>=
<
<=

//Then we have this dumb fucking stupid fucking thing
is
```

`is` compares the "boxes" of 2 variables.

```
list1=["string1"]
list2=["string1"]


if list1 == list2:
	print("lists are the same")
//=> "lists are the same"


if list1 is list2:
	print("True!")
else:
	print("They are not IS")
//=> "They are not IS"

duplicatelist = list1

if list1 is duplicatelist:
	print("True!")
else:
	print("They are not IS")
//=> "True!"
// This returns true because duplicatelist becomes a pointer to list1, therefore it IS the same thing (in literal memory)

```


**and/or**/not

instead of using &&, || and !, we use the literal word "and", "or" and "not"

```
if x==10 and y==5:
	print("This only prints if BOTH x and y are the respective values")


if x==10 or y==5:
	print("This will print if EITHER x or y are the respective values")

if not x==10 and not y==5:
	print("This will print if NEITHER x or y are the respective values")
```



#### Loops

```
for x in range(20):
	print(f"The value of X is {x}")


fruits = ["bananana", "apple", "orange", "grape"]

for fruit in fruits:
	print(f"The current fruit we're looping over is is {fruit}")

count = 10
while count>=0:
	print(count)
	countdown -= 1
	
//=> 10 9 8 7 6 5 4 3 2 1 0

```


#### Functions

we use the "def" keyword rather than "function"

Functions are *not* hoisted in python. We can only call them after they've been defined

```
def functionName(argument1, argument2):
	print(argument1, argument2)
	return argument1+argument2
```


#### Error Handling
```
try:
	print("trying")
except Exception as e:
	print("Failed! Erorr here " + e)
	
```

```
raise ValueError("errrororororor value erro")
```


#### List comprehension
Similar to mapping in JS

```
newList = [(ANY EXPRESSION USING X) for (X) in (ORIGINAL LIST)]

// Any expression is just any expression - we can do math, type conversion, call // functions, etc on our variable (X). (X) is just a for loop on the original list


list_example = [2, 4, 6, 8, 10]

example = [x*2 for x in list_example]

print(example)
//=> 4, 8, 12, 16, 20


// Calling a function with list comprehension

def square(x):
	return int(x) * int(x)

list_example = [2, 4, 6, 8, 10]

const squared = [square(x) for x in list_example]

print(squared)
//=> 4, 16, 36, 64, 100

```

#### In

Way to check if an element is a part of a list.

```
fruits = ["orange", "pear", "strawberry"]

if "orange" in fruits:
	print("We have an orange!")
```

#### Import/Exports

```
//file1.py
def squareFunction(x):
	return int(x) * int(x)



//file2.py

from file1 import squareFunction

print(squareFunction(2))
```









