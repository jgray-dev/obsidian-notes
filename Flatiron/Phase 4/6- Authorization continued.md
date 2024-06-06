
#### Review

Basically this is how we handle login, sessions, and logout
```python
#
# Here we set valid routes that a user can access without being logged in, these
# are unprotected routes that we don't really care about
#
@app.before_request
def check_credentials():
    valid_routes = ("/check_sessions","/login")
    # If the route is not in valid routes but the user is logged in 
    if request.path not in valid_routes and 'user_id' not in session:
        return {"error": "please login"},401
    else:
        pass


#
# Login route will set the session user_id to our logged in user. We should also
# check password here before setting the session, but it wasn't covered in the
# lecture (it will be covered below)
#
@app.route('/login', methods = ["POST"])
def login():
    data = request.get_json()
    user = User.query.filter(User.username == data["username"]).first()
    if user:
        session["user_id"] = user.id
        return user.to_dict()
    else:
        return {"error":"Cannot login"},400

#
# Here we check the session, we call this in app.jsx in a useEffect, to set our
# user on the client side
#
@app.route('/check_sessions')
def check_sessions():
    if session.get("user_id"):
        user = User.query.filter(User.id == session.get("user_id")).first()
        return user.to_dict()
    else:
        return {"error": "no user logged in"},401


#
# Simple route to logout and clear our session's user_id. We would also need to
# reset the user on the client side
#
@app.route('/logout', methods=["DELETE"])
def logout():
    session.pop('user_id')
    return {}, 204


#
# Example protected route, after a certain amount of "reads", the user is unable
# to view this route anymore. Example of a very very basic paywall
#
@app.route('/blog/<int:id>')
def get_blog(id):
    if not session.get('reads'):
        session['reads'] = 0
    session['reads'] += 1
    if session['reads'] <= 5:
        return {
            "id": id,
            "data":"This is a blog"
            }
    else:
        return {'error': 'pay me monies'},400
```

****
#### New shizzzz

To correctly add a password, we need to hash it with salt.

`pipenv install flask-bcrypt`

```python
import Flask 
#...
from app import bcrypt
# Models.py

class User(db.Model,SerializerMixin):
    __tablename__ = 'users'
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String, unique = True)
    created_at = db.Column(db.DateTime, server_default=db.func.now())
    updated_at = db.Column(db.DateTime, onupdate=db.func.now())
    
	password_hash = db.Column(db.String)
	
	@hybrid_property
	def password_hash(self):
		return self._password_hash


```

bcrypt needs access to our app when we init it, so David recommends we create a new file, `services.py`

in this file we do all our setup/boilerplate stuff

```python
from flask_sqlalchemy import SQLAlchemy
from sqlalchemy.orm import validates
from sqlalchemy import MetaData
from sqlalchemy_serializer import SerializerMixin
from flask import Flask, request, make_response, jsonify, session
from flask_migrate import Migrate
from flask_restful import Api, Resource
from flask_cors import CORS
import os
from dotenv import load_dotenv

metadata = MetaData(naming_convention={
    "ix": "ix_%(column_0_label)s",
    "uq": "uq_%(table_name)s_%(column_0_name)s",
    "ck": "ck_%(table_name)s_`%(constraint_name)s`",
    "fk": "fk_%(table_name)s_%(column_0_name)s_%(referred_table_name)s",
    "pk": "pk_%(table_name)s"
    })

db = SQLAlchemy(metadata=metadata)

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///app.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
app.json.compact = False

migrate = Migrate(app, db)
db.init_app(app)

api = Api(app)
CORS(app)

load_dotenv()
app.secret_key = os.getenv('secret_key')

```

This way we're able to (from `app.py`, or `models.py`) `from services import *` so we're not in an endless importing loop, where models imports from app, app imports from models, etc etc etc.

Now app imports from services, and models imports from services.
Because services doesn't need to import anything from anywhere, we prevent endless loops.


```python
class User(db.Model,SerializerMixin):
    __tablename__ = 'users'
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String, unique = True)
    created_at = db.Column(db.DateTime, server_default=db.func.now())
    updated_at = db.Column(db.DateTime, onupdate=db.func.now())
    
	password_hash = db.Column(db.String)
	
	@hybrid_property
	def password_hash(self):
		return self._password_hash
		
	@password_hash.setter
	def password_hash(self, password):
		hash = bcrypt.generate_password_hash(password.encode('UTF-8'))
		self._password_hash = hash.decode('UTF-8')
		
	def check_password(self, password):
		return bcrypt.checkpw(self._password_hash, password.encode('UTF-8'))
```

Now in our login route:

```python
@app.route('/login', methods = ["POST"])
def login():
    data = request.get_json()
    user = User.query.filter(User.username == data["username"]).first()
    if user:
	    if user.check_password(data['password']):
	        session["user_id"] = user.id
	        return user.to_dict()
	    else:
		    return {"error": "Incorrect password"},400
    else:
        return {"error": "User not found"},404
```


We should also implement a "stay logged in" checkbox input in our frontend. Not showing frontend code cuz its easyyyyyyyyyyyyyyy but in our backend:

```python
@app.route('/login', methods = ["POST"])
def login():
    data = request.get_json()
    user = User.query.filter(User.username == data["username"]).first()
    if user:
	    if user.check_password(data['password']):
	        return user.to_dict()
			if data['stay_signed_in']:   # **ADD THIS LINE**
		        session["user_id"] = user.id
	    else:
		    return {"error": "Incorrect password"},400
    else:
        return {"error": "User not found"},404

```












