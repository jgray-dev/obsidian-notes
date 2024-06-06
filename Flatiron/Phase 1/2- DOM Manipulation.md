The Document Object Model (DOM) allows you to interact with and modify the HTML structure of a web page.

`const heading = document.querySelector('h1'); heading.textContent = 'New Heading';`


```
const newDiv = document.createElement('div'); 
newDiv.textContent = 'This is a new div'; document.body.appendChild(newDiv);
````