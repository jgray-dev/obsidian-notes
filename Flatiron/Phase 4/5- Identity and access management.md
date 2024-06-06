


```python
#Import session
from flask import Flask, request, make_response, jsonify, session
```


#### .env


`pipenv install python-dotenv`
Create a .env file in our root directory
Make sure we put this file in our .gitignore to keep it away from github

```python
secret_key="blahblahblah"
```


back in our app, we must

`os = load_dotenv()`

then we can call
```python
import os
import dotenv from 

os = load_dotenv()

print(os.getenv('secret_key'))
# => "blahblahblah"
```


#### Sessions

Used to save accounts, track users, and farm data :nerd:

Sessions are treated like dicts, so we're able to access and declare specific data within them using the basic dictionary notation


```python
@app.route('/login/')
def login_page():
	session['login'] = True
	return {}

@app.route('/checklogin')
def check_session():
	print(session)
	return {}
```

Now when the user goes to the `/login` route, Flask will set the session "login" to true. When the user then goes to `/checklogin`, it will print our session to our python console

```python
# /checklogin
# => <SecureCookieSession {}>


# /login => /checklogin
# => <SecureCookieSession {'login': True}>
```

To get sessions to work, we need to setup our proxy. Proxy should ideally work for everything, and allows us to just fetch `/login`, rather than `http://67.164.191.36:5000/login`

```json
// package.json
{
  "name": "reactvitetailwind",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "proxy": "http://67.164.191.36:5000", //   ** ADD THIS LINE **
  "scripts": {
    "start": "vite --port 3000 --host",
	// ...
  },
  "dependencies": {
    "@headlessui/react": "^1.7.18",
	// ...
  },
  "devDependencies": {
    "@types/react": "^18.2.64",
	// ...
  }
}
```


Actually using data now

```python
@app.route('/login/' methods=['POST'])
def login_page():
	username = request.json['username']
	session['login'] = username
	return {}

@app.route('/checklogin')
def check_session():
	print(session)
	return {}
# /login (Posted with username = Jackson) => /checklogin
# =>  <SecureCookieSession {'login': "Jackson"}>
```

We're able to link these routes directly to our database, to check if a user exists, and only return and set the session user if it's real.

Then in our frontend we can really build our login using a form.


****


```python
@app.route('/login/' methods=['POST'])
def login_page():
	username = request.json['username']
	user = User.query.filter(User.username == request.json('username')).first()
	if user:
		session['username'] = username
		return {},200
	else:
		return {},400

@app.route('/checklogin')
def check_session():
	if session.get('username')
		user = User.query.filter(User.username == session.get('username')).first()
		return user.to_dict(), 200
	else:
		return {},401
```


****
#### Logouts


```python
@app.route('/login/' methods=['POST'])
def login_page():
	username = request.json['username']
	user = User.query.filter(User.username == request.json('username')).first()
	if user:
		session['username'] = username
		return {},200
	else:
		return {},400

@app.route('/checklogin')
def check_session():
	if session.get('username')
		user = User.query.filter(User.username == session.get('username')).first()
		return user.to_dict(), 200
	else:
		return {},401

@app.route('/logout', methods=['DELETE'])
def route_logout():
	session['username'] = None # To just set the k/v pair to nothing
	# OR
	session.pop('username') # to completely remove the username k/v pair
	return {}, 200
```


With this backend, we're able to POST to our `/login` route with a username in the body, check the current session user via the `/checklogin`, and finally we have a route to set our session username to nothing, AKA log out, through our `/logout` route


Sessions are apparently very secure. **User's shouldn't be able to tamper with our session data regardless of the circumstance**

Paywalls after a certain about of reads, 



























