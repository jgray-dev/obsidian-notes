
David's example summed up:
```java
davids_cat = "Midna"

def zoomies(cat):
	print(f"{cat} has the zoomies")

def feed_cat(cat):
	print(f"{cat} is eating")
// Now that we're feeding our cat, it makes sense to keep track of our cat's hunger. Same if we made a function for cat drinking, we need to keep track of its thirst, etc etc etc. To solve this we'd change davids_cat into a object/dictionary that includes name, hunger, thirst, etc.
```


With OOP, we can create out own class to make davids_cat's attributes reusable, so we can create multiple cats, with different names, hunger, and thirst levels

Basic syntax includes the keyword "Class", with the name of our new class (Title case is preferred)

Then we make the initialization function, with the arguments "self", and any attributes/variables we want to keep for each cat. 

The arguments can have default values, in this case we default everything to 10

```java
class Cat:
	def __init__(self, name, hunger=0, thirst=0, energy=0):
		self.name = name
		self.hunger = hunger
		self.thirst = thirst
		self.energy = energy


// To create a new Cat object:
// We ignore the "self" variable (in this case, lyla becomes "self")
// When init a new object, we can pass any data types as variables, this case we're mixing a string and integers

lyla = Cat("Midna", 100, 50, 100)

print(lyla.energy)
```

```
print(type(lyla))
//=> <class '__main__.Cat'>

//Cat is now our own type
```


This is when our stupid ahh `is` keyword comes in

```
if type(lyla) is Cat:
	print("Lyla is a cat")
```



Nobody knows what repr stands for, but its a method we can use it to return a value when we try to print a type of class

```java
class Cat:
	def __init__(self, name, hunger=0, thirst=0, energy=0):
		self.name = name
		self.hunger = hunger
		self.thirst = thirst
		self.energy = energy
	def __repr__(self):
		return f"{self.name} has a hunger of {self.hunger}, thirst {self.thirst}, energy {self.energy}"


lyla = Cat("Lyla", 100, 0, 100)
print(lyla)
//=> Lyla has a hunger of 100, thirst 0, energy 100

```



#### Custom methods:

```java
class Cat:
	... init and repr here ...
	
	def feeding(self, amount):
		print(f"Feeding {self.name}")
		if self.hunger > amount:
			self.hunger -= amount
			self.drinking(10)
			//If we call other methods, we need to use dot notation before calling the method
	
	def drinking(self, amount):
		print(f"Feeding {self.name}")
		if self.hunger > amount:
			self.hunger -= amount

lyla = Cat(.....)

lyla.feeding(35)
// Would remove 35 from lyla's hunger, assuming her hunger is above the 35 amount // argument we pass in
```


### Properties (Basically encapsulation) 
(getter + setter)


If we want a setter function to hide logic, we **must** have a getter method as well 
```java
class Cat:
	... init, repr, feeding, drinking here ...
	def get_name(self):
		return self._name
	def set_name(self,value):
		if len(value) > 3:
			self._name = value
		else:
			raise ValueError("invalid name (too short)")
	name = property(get_name, set_name)
```

Breaking it down:

This is basically encapsulation to hide the logic behind setting our name, behind the Cat class. So if we wanted to check name length before setting it, or check another cat with that name doesn't exist, we put that logic in our setter function, so we don't need to add the logic every time we change names or accept inputs, etc.

Using `self._name` instead of `self.name` prevents us from running into an infinite loop. IT "bypasses" the getter or setter property, because when returning `self.name` (or setting it), we're calling the getter (or setter) property, returning (setting) the value, calling the property, etc etc etc

##### Global Variables

We can keep a list of all our cats (if we have multiple), to prevent name conflicts, and give us reference to what "cats" actually exist 

```java
class Cat:
	all_cats = []
	def __init__:
		... exisiting logic ...
		Cat.all_cats.append(self)
```


#### Class methods
With this `all_cats` list, we can make new methods such as `feed_all_cats`:
To do this, we need to add a "decorator" `@classmethod` so Python knows (*clown emoji here*) its a method to be applied class-wide

```java
class Cat:
	... ... ...
	@classmethod
	def feed_all_cats(cls):
		for cat in cls.all_cats:
			cat.feeding(25)
			print(f"{cat} has been fed 25")
```

Then call it with `Cats.feed_all_cats`

`(cls)` stands for "class" and we need it in order to use our class level variable, `all_cats`. Can be named anything, but its a reference to the class, and we don't want to nest duplicate keywords, so best practice is basically `cls`

```java
class Cat:
	... ... ...
	@classmethod
	def find_cat(cls, cat_to_find):
		for cat in cls.all_cats:
			if cat is cat_to_find:
				print(f"{cat} has been found")
```