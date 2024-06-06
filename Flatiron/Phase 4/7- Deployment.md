```python
@app.errorhandler(404)
def 404():
	return render_template("index.html")
```



```python
pipenv install psycopg2-binary
pipenv install gunicorn
```

