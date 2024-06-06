The `fetch` function is used to make HTTP requests to servers and retrieve data.

```
fetch('https://api.example.com/data')
	.then(response => response.json()) 
	.then(data => console.log(data))
	.catch(error => console.error(error));
```
