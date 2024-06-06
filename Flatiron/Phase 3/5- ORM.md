
``` python
import sqlite3

connection = sqlite3.connect("post.db")
cursor = connection.cursor()
```


Using a class 

To use a class for CRUD in a SQL database
This is assuming we've created a table called "posts" with columns `id` and `title`
```python
import sqlite3

connection = sqlite3.connect("post.db")
cursor = connection.cursor()

class Post:
	def __init__(self, title, id=None):
		self.title = title
		self.id = id
		
	def save(self):
		new post = cursor.execute(
			f'''
			INSERT INTO posts(title)
			VALUES("{self.title}");
			'''
		)
		connection.commit()
		
	def delete(self):
		cursor.execute(
			f'''
			DELETE FROM posts
			WHERE id = {self.id}
			'''
		)
		connection.commit()

	def patch(self, new_title):
		cursor.execute(
			f'''
				UPDATE posts
				SET title = "{new_title}"
				WHERE id = {self.id}
			'''
		)
		connection.commit()

	@classmethod
	def get_by_id(cls, search_id):
		data = cursor.execute(
			f'''
			SELECT * FROM posts
			WHERE id = {search_id}
			'''
		).fetchone()
	
		if data: # If we have data, return it as an instance of a Post
			return Post(data[0], data[1])
	
	@classmethod
	def get_all(cls):
		data = cursor.execute(
			f'''
			SELECT * FROM posts
			'''
		).fetchall()
		posts = list()
		for post in data:
			posts.append(Post(post[0], post[1]))
		return posts
```


****
Using Faker to fake data

`pipenv install Faker`
``` python
from faker import Faker
faker = Faker()


faker.name()
//=> "Sarah Tucker"
faker.name()
//=> "John Adams"


for x in range(10):
	cursor.execute(
		'''
		INSERT INTO students(name, email)
		values(?,?)
		''',
		(faker.name(), faker.email())
	)
```



