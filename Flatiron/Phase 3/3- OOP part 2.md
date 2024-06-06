#### OOP Relationships
one-to-one, one-to-many, many-to-many


## one-to-one
```python
class Car:
	def __init__(self, make, model):
		self.make = make
		self.model = model
		self.driver = None
	def drive(self, distance):
		print(f"{self.driver} drives his/her {self.model} {distance} miles")

class Driver:
	def __init__(self, name, age):
		self.name = name
		self.age = age
		self.car = None
		
	def drive_car(self, car):
		self.car = car
		car.driver = self


jackson = Driver("Jackson", 20)

jackson.drive_car(Car("BMW", "M3"))
// We can create the car inline in the function that needs the car. We could also create the car object outside of where we call it, but we'd need to add a method to creating the object that sets the driver to something other than None (right now its only being set via the drive_car method under our Driver class)
```

When we link them like this, adding each class to each other, the Jackson driver object gets access to all the car methods, by using `jackson.car.(WHATEVER)` so we can now do `jackson.car.drive`despite Jackson not actually having the drive method attached to its type - its a method of the Car object


## one-to-many

```python
class Zoo:
	def __init__(self, name):
		self.name = name
		self.animals = []
		
	def add_animals(self, animals):
		for animal in animals:
			self.animals.append(animal)
			animal.zoo = self
			print(f"{animal.species} has been added to {self.name}")
	def feed_all(self):
		//Now our zoo.animals is set to a list, we can use this list as our "master" list of animals
		for animal in self.animals:
			print(f"{animal} is being fed")
	

class Animal:
	def __init__(self, species):
		self.species = species
		self.zoo = None

denver_zoo = Zoo("Denver Zoo")

animal1 = Animal("Tiger")
animal2 = Animal("Elephant")
animal3 = Animal("Ferret")

denver_zoo.add_animals([animal1, animal2, animal3])
//=> Tiger has been added to Denver Zoo
//=> Elephant has been added ...
//=> ...
```

By setting `zoo.animals` to an empty list (array), we imply that every Zoo object will have *multiple* animal "children" (these are NOT parent/children components, its just easy to think of it like this for familiarity)

## many-to-many

```python
class Bar:
	def __init__(self, name):
		self.name = name
		self.cocktails = []
	def add_to_menu(self, cocktails):
		for cocktail in cocktails:
			self.cocktails.append(cocktail)
			cocktail.bars.append(self)
	

class Cocktail:
	def __init__(self, name):
		self.name = name
		self.bars = []
	def add_to_bar(self, bar):
		print(f"adding {self.name} to {bar}")
		bar.cocktails.append(self)

bar1 = Bar("Cherry Cricket")
bar2 = Bar("Viewhouse")

ct1 = Cocktail("Straight vodka shot")
ct2 = Cocktail("Straight tequilla shot")
ct3 = Cocktail("soju shot")

bar1.add_to_menu([ct1, ct2])
bar2.add_to_menu([ct1, ct2, ct3])

```


#### "Join tables"

Instead of a many-to-many relationship, 
we have a
one to many relationship from bar -> menu
and a one to many relationship to menu from cocktail

bar -> menu <- cocktails

```

```

I had to step out for this portion of lecture, but the in class code challenge is here:

```python
class Instructor:
    all = []

    def __init__(self, name, email):
        self.name = name
        self.email = email
        Instructor.all.append(self)

    def students(self):
        students_list = []
        for cs in Class_Schedule.all:
            if cs.teacher is self:
                students_list.append(cs.student)
        return students_list

    def grade_student(self, student, grade):
        for cs in Class_Schedule.all:
            if cs.student is student and cs.teacher is self:
                cs.grade = grade


class Class_Schedule:
    all = []

    def __init__(self, title, period, teacher, student):
        self.title = title
        self.period = period
        self.grade = 0
        self.teacher = teacher
        self.student = student
        Class_Schedule.all.append(self)


class Student:
    all = []

    def __init__(self, name, email):
        self.name = name
        self.email = email
        Student.all.append(self)

    def join_class(self, title, period, teacher):
        Class_Schedule(title, period, teacher, self)

    def teachers(self):
        tl = []
        for cs in Class_Schedule.all:
            if cs.student is self:
                tl.append(cs.teacher)
        return tl

    def grade(self):
        classes = []
        for cs in Class_Schedule.all:
            if cs.student == self:
                classes.append(cs.grade)
        return sum(classes) / len(classes)


david = Instructor("David", "d@flatiron.com")
stephen = Instructor("Stephen", "s@flatiron.com")
zac = Student("Zac", "z@gmail.com")
sam = Student("Sam", "s@gmail.com")
kaleab = Student("Kaleab", "k@gmail.com")

zac.join_class("css 101", 1, david)
sam.join_class("css 101", 1, david)
kaleab.join_class("css 101", 1, david)
zac.join_class("react 205", 2, stephen)
sam.join_class("react 205", 2, stephen)

david.grade_student(zac, 4)
stephen.grade_student(zac, 2)

print(zac.grade())
```

The relationship looks like this

`teacher -> class schedule <- students`

teacher is a one to many class schedule, and students are a one to many class schedule.



## Inheritance

We use super(). to set the properties of the parent object

```python
class Animal:
	def __init__(self, name, color):
		self.name = name
		self.color = color

class Cat(Animal):
	def __init__(self, name, color, breed):
		super().__init__(name, color)
		self.breed = breed

class Dog(Animal):
	def __init__(self, name, color):
		super().__init__(name, color)
```

Here, our dog is just a copy of our Animal class, but Cat has an additional argument (breed)

We can duplicate this code for other animals that require different arguments, and add all similarities into the super Animal class



```python
class Animal:
	def __init__(self, name, color):
		self.name = name
		self.color = color
	def pet_animal(self):
		print(f"petting {self.name}")

class Cat(Animal):
	...

class Dog(Animal):
	...
		
class Bird(Animal):
	def __init__(self, name, color):
		super().__init__(name, color)
	pet_animal(self):
		print(f"{self.name} flies away!!")
```

We can overwrite super methods by just redefining them in the child class. In this case, petting a dog or cat will print `petting {name}`, but petting a bird will print `{name} flies away!!`




