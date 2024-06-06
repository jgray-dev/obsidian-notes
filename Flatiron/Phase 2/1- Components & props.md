App.js is our 'home' file. Components and HTML will pass through here to be rendered on the page

**Components**:
- Create a function in a new file
- Component name must start with a capitol letter - Use lowercase for regular functions
- Must export our component (export default Header) before it can be used elsewhere
- Must import our component (import Header from './Header') before it can be used
(!) When returning HTML/JSX from a component, ALL HTML element must be lowercase - react will interpret capital's as react components  

```
function Header() {
	return (
	<header>This is our header</header>
	<button onClick={clickEvent}>This is a button with a click event</button>
	)
}

function clickEvent() {
	console.log("Element was clicked")
}

export default Header
```

**Props**:
- Variables that we pass into our function. We can only pass ONE 'prop' into a function
- Pass an object with key value pairs to pass multiple variables as a prop

```
// Header.js
function Header(props) {
	return (
	<header>This is our header {props.name} is our name. Our otherVariable is {props.otherVariable}</header>
	)
}

export default Header
```


```
//App.js (or wherever we're using our component)
import Header from "./Header"

// Note the capitol H
<Header name={"jackson"} otherVariable={9102} />
```

Props can be passed as a props object, or "destructured" variables. (Imagine its just destructing the prop object into its individual elements)

```
//Header.js
function Header({name, otherVariable}) {
	return (
	<header>This is our header {name} is our name. Our otherVariable is {otherVariable}</header>
	)
}

export default Header

//App.js
<Header name={"jackson"} otherVariable={9102} />

```
The order of destructured variables do not matter, provided the variable names match ***exactly***



















