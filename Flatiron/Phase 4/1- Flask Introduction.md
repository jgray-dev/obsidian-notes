
Packages we should be using:

```python
flask
flask-sqlalchemy
flask-migrate
sqlalchemy-serializer
```

```python
from flask import Flask, jsonify, make_response, request
from flask_migrate import Migrate


app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///app.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
migrate = Migrate(app,db)
db.init_app(app)

```

From console:
`export FLASK_APP=<PATH TO APP.PY>`
`export FLASK_RUN_PORT=<PORT>`
`flask db init`
`flask db migrate -m <MESSAGE>`
`flask db upgrade`

```python
export FLASK_APP=backend/app.py
export FLASK_RUN_PORT=5555
flask db init
flask db migrate -m "create new table blah blah blah"
flask db upgrade
```


****
#### Creating a database table in Flask/SQLAlchemy

```python
# models.py
metadata = MetaData(naming_convention={ "ix": "ix_%", "uq": "uq_%", "ck": "ck_%", "fk": "fk_%", "pk": "pk_%" })


db = SQLAlchemy(metadata=metadata)

class Yoyo(db.Model):
	__tablename__ = 'yoyos'
	id = db.Column(db.Integer, primary_key=True)
	rpm = db.Column(db.Integer)
	company = db.Column(db.String)
	size = db.Column(db.String)
```
This is basically creating the database for us.
We can modify a table by simply modifying this class that inherits from `db.Model`, and run the command `flask db migrate -m <message>` followed by `flask db upgrade` to "push" the changes to our database. The `flask db` commands are similar to git version control.



****
#### App.py boilerplate config
```python
from flask import Flask, jsonify, make_response, request
from flask_cors import CORS #For me i need to import CORS because im hosting it from home.
from flask_migrate import Migrate
from models import db, Yoyo


app = Flask(__name__)
CORS(app) #For me i need to specify CORS because im hosting it from home.
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///app.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
migrate = Migrate(app,db)
db.init_app(app)


#routes, functions, blah blah blah


#For me i need to specify host=0.0.0.0 because im hosting it from home.
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000, debug=True)
```


****
#### Seed file basic syntax/guide
```python
#seed.py
from app import app
from models import db, Yoyo


#This is our seed file, so we wipe content and re-fill after
with app.app_content():
	Yoyo.query.delete()  # Delete our table data
	y1 = Yoyo( #Create a new Yoyo instance
		rpm = 100,
		company = "Yoyo Inc",
		size = 1
	)
	db.session.add(y1) # Add to our db
	db.session.commit # Commit our changes
```


****
#### Full seed file
```python
#seed.py
from app import app
from models import db, Yoyo


with app.app_content():
	Yoyo.query.delete()
	y1 = Yoyo(
		rpm = 100,
		company = "Yoyo Inc",
		size = 1
	)
	y2 = Yoyo(
		rpm = 50,
		company = "Yoyo Inc",
		size = 6
	)
	y3 = Yoyo(
		rpm = 2000,
		company = "Yoyo and company",
		size = 2
	)
	db.session.add_all([y1, y2, y3])
	db.session.commit
```



****
#### Routes

```python
#app.py

from flask import Flask, jsonify, make_response, request
from flask_migrate import Migrate
from models import db, Uoyo

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///app.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
migrate = Migrate(app,db)
db.init_app(app)


@app.route('/')
def home_page():
	return 'URL undefined woahh'

@app.route('/yoyos')
def get_yoyos():
	all = Yoyo.query.all()
	
	return [{
	"id": yoyo.id,
	"rpm": yoyo.rpm,
	"company": yoyo.company,
	"size": yoyo.size
	}for yoyo in all]
	# Use list comprehension to return a serializeable object.




# DYNAMIC URLS 
# ** This would break if run because our functions (def title) are all called the same thing, but the essence remains the same**
@app.route('/<title>')
def title(title):
	return f"the title is: {title}"

#We can have as many dynamic variables as we'd like
@app.route('/<title>/<author>')
def title(title, author):
	return f"the title is: {title} and the author is {author}"

#We can specify datatypes in a route dynamic URL aswell
@app.route('/<str:title>/<str:author>/<int:id>')
def title(title, author):
	return f"the title is: {title} and the author is {author}. The ID is an integer: {id}"

```

These dynamic routes are fking amazing because in my phase 3 project, because I had to use `POST` requests whenever I wanted to search if a username existed for example, but now, I can use a `GET` request and specify the data in the URL. We should specify the data type of our dynamic URLs, otherwise it will default to a string and we'll need to do type conversion and sanitization.


```python
#app.py
@app.route('/yoyos/<int:id>')
def get_yoyo():
	yoyo = Yoyo.query.filter(Yoyo.id == id).first()
	if yoyo:
		return {
			"id": yoyo.id,
			"rpm": yoyo.rpm,
			"company": yoyo.company,
			"size": yoyo.size,
			"ok": True
		}
	else:
		return {
			"error": "Yoyo ID not found!",
			"ok": False
		}, 400
	
```


****

#### Validate models

```python
# seed.py
#... metadata imports blah blah blah

class Yoyo(db.Model):
	__tablename__ = 'yoyos'
	id = db.Column(db.Integer, primary_key=True)
	rpm = db.Column(db.Integer)
	company = db.Column(db.String)
	size = db.Column(db.String)
	era = db.Column(db.Integer, nullable = False)
	@validates("era")
	def validate_era(self, key, value):
		valid = (1970, 1980, 1990, 2000, 2010, 2020)
		if value in valid:
			return value
		else:
			raise ValueError("Invalid era")
```


```python


#Before request typically used to protect data is the user is not logged in properly. Returns given here overwrite whatever route actully got called. In this case, if loggedin is false, the only thing the user can access is our error statement. If they are logged in, they will be allowed to reach the requested protected endpoint 
@app.before_request
def before():
	print("this prints before EVERY request we get")
	print(request.path) # Will print whatever route we're calling.
	loggedin = True
	if not loggedin:
		return {"Error": "Please sign in first", "ok": False}, 401



```


****
#### Flask methods
```python
@app.before_first_request

@app.before_request 

@app.after_request # REQUIRES a argument that is by default the returned content. Only triggers if the return is successful with no errors

@app.teardown_request # Triggers after a function is called, regardless of an error. Requires argument corresponding to return of the route called

```
