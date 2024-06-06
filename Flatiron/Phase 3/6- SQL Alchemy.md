

Creating classes linked with a db in alchemy is much easier. We don't do the whole "init" thing, we just define the columns within a class

`pipenv install sqlalchemy`


#### Basics
``` python
from sqlalchemy import Column, Integer, String, create_engine, func
from sqlalchemy.orm import Session, declarative_base, validates

Base = declarative_base()

class Trail(Base):
	__tablename__ = "trails"
	id = Column(Integer, primary_key = True)
	name = Column(String)
	location = Column(String)


if __name__ == "__main__":
	# SQLAlchemy makes it easy to init engines from different tools, such as
	# postgres,  we just replace "sqlite" with "postgres"
	engine = create_engine('sqlite:///database.db')

	Base.metadata.create_all(engine)
```

We define the table name we're a part of with `__tablename__`
and add all of the columns with plain variables (id, name, location)
we initialize the sql engine under the `__main__` line, and `create_all` using that engine
#### Adding data
``` python
if __name__ == "__main__":
	# . . . (engine, create_all)
	# . . .
	
	with Session(engine) as session:
		t1 = Train(
		name = "Chitaqua"
		location = "Coulder"
		)
		session.add(t1)
		session.commit()
		# To add multiple:
		t2 = Trail(
			#...
			#...
		)
		session.add_all([t1, t2]) # Pass an array of Trail classes to add_all

```

This is how we add things to our newly created database. We just create a new instant of our `Trail` class, add it to our session, and commit the session

##### Querying data from our table
```python
if __name__ == "__main__":
	# . . . (engine, create_all)
	# . . .
	
	with Session(engine) as session:
		# create trais, add trails blah blah blah

		all_trails = session.query(Trail).filter(Trail.name == "NAME").all()
		print(all_trails)
		# => Prints the weird <Trail 0xFF02983472> bullshit unless we add a 
		#    repr lol
		all_trails = session.query(Trail).order_by(Trail.name.asc()).all()
		# => Returns us a sorted list of our rows in our database
```


### Modifying a table

##### Dropping a table
```python
if __name__ == "__main__":
	# create engine create all blah blah blah . . .
	Trail.__table__.drop(engine)
```
we call `drop()` on the table we'd like to remove, and pass our engine as an argument


##### Editing a row:

```python
if __name__ == "__main__":
	#. . .	
	with Session(engine) as session:
		i = input("Trail name:")
		selected = session.query(Trail).filter(Trail.name == i).first()
		new = input("New trail name:)
		selected.name = new
		session.add(selected)
		session.commit()
```

This will alter our Trail name, taking our original trail name as the first input, and a new name as the second input. We only need to update whatever we're changing (`selected.name = new`) because alchemy knows when we commit it, that the ID already exists, and only modifies the existing row, rather than creating a new row.


##### Deleting rows
```python
if __name__ == "__main__":
	#. . .	
	with Session(engine) as session:
		i = input("Trail name:")
		selected = session.query(Trail).filter(Trail.name == i).first()
		selected.delete()
		session.commit()
```

pretty self explanatory lol

****
### Properties (Validates)
```python
class Trail(Base):
	__tablename__ = "trails"
	id = Column(Integer, primary_key = True)
	name = Column(String)
	location = Column(String)

	@validates("name")
	def validate_name(self, key, value):
		if type(value) is str:
			return value
		else:
			raise ValueError("Name must be a string")
			
```

the `key` argument in `validate_name()` makes `@validates` very powerful for us

It changes based on the column we're attempting to change in our DB

```python
class Trail(Base):
	#...
	@validates("name", "location")
	def validate_name(self, key, value):
		if type(value) is str:
			if key == "name" and len(value) > 4:
				return value
			elif key == "location" and len(value) > 6:
				return value
			else:
				raise ValueError("*Loud buzzer sounds*")
		else:
			raise ValueError("Name must be a string")
```

We're able to have multiple `@validates` just like properties in ORM methods, but if we have multiple values that require the same restrictions (must be string, length > 3, etc etc etc) it makes much more sense to use `@validates` with multiple arguments.

We can use the key to use different parameters, in the example above we're requiring a different length for each `name` and `location` values, but we only need to write our string check once because our `@validates` is handling both of them


****

### Connecting tables

```python
class Trail(Base):
	__tablename__ = "trails"
	id = Column(Integer, primary_key = True)
	name = Column(String)
	location = Column(String)
	mountain_id = Column(Integer, ForeignKey('mountains.id'))
	# 'mountains' is our table name, id is our column we're referencing
	mountain = relationship("Mountain", back_populates="trails")

class Mountain(Base):
	__tablename__ = "mountains"
	id = Column(Integer, primary_key = True)
	name = Column(String)
	trails = relationship("Trail", back_populates="mountain")

if __name__ == "__main__":
	engine = create_engine('sqlite:///database.db')
	Base.metadata.create_all(engine)
	with Session(engine) as session:
```

```python
x = relationship("CLASSNAME", back_populates="y")
y = relationship("CLASSNAME", back_populates="x")
```