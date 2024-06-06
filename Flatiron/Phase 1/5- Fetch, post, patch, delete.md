In addition to `GET` requests, you can use `fetch` to send `POST`, `PATCH`, and `DELETE` requests to create, update, and delete server resources.

const newData = { name: 'John Doe', age: 30 };

```
fetch('(https://api.example.com/users)', { 
	method: 'POST', 
	headers: { 
	'Content-Type': 'application/json' 
	},
	 body: JSON.stringify(newData) 
 }) 
	 .then(response => response.json())
	 .then(data => console.log(data))
	 .catch(error => console.error(error));
```
