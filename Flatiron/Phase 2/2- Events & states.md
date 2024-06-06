#### **Events**

Events are similar to JavaScript Event Listeners, but use a different syntax and have different rules.
- They may **NOT** be attached to a react component. Only HTML elements may contain a react event handler (ironic)
- Uses a different inline syntax, rather than dot notation like JS
````
	return <button onClick={handleClickFunction}>Tickle me!</button>;
````
This is part of a React component, but we can **ONLY** add the event handler to the native HTML button element. 


````
	return <Clickable onClick={handleClick} />;
````
**(!) (!) (!) THIS WOULD NOT WORK (!) (!) (!)** 
<Clickable /> is a react component, therefore we cannot add an event to it!

To pass a variable to our callback event function, we must use arrow notation to prevent the function from being immediately invoked when rendered:
```
	<button onClick={() => handleClick(1)}>Button 1</button>
```


#### **Types of react events**

1. **onClick**: Triggered when the user clicks on an element (e.g., a button or a link).
2. **onChange**: Triggered when the value of an input, textarea, or select element changes.
3. **onSubmit**: Triggered when a form is submitted.
4. **onBlur**: Triggered when an element loses focus.
5. **onFocus**: Triggered when an element gains focus.
6. **onKeyDown**: Triggered when a key is pressed down.
7. **onKeyUp**: Triggered when a key is released.
8. **onKeyPress**: Triggered when a key that produces a character value is pressed down.
9. **onMouseOver**: Triggered when the mouse pointer moves over an element.
10. **onMouseOut**: Triggered when the mouse pointer moves out of an element.
11. **onMouseEnter**: Triggered when the mouse pointer enters an element.
12. **onMouseLeave**: Triggered when the mouse pointer leaves an element.
13. **onMouseDown**: Triggered when the mouse button is pressed down on an element.
14. **onMouseUp**: Triggered when the mouse button is released on an element.
15. **onMouseMove**: Triggered when the mouse pointer moves within an element.
16. **onScroll**: Triggered when an element's scrollbar is being scrolled.
17. **onDragStart**: Triggered when the user starts dragging an element or text selection.
18. **onDragEnd**: Triggered when the user stops dragging an element or text selection.
19. **onDragOver**: Triggered when a dragged element or text selection is moved over a valid drop target.
20. **onDragEnter**: Triggered when a dragged element or text selection enters a valid drop target.
21. **onDragLeave**: Triggered when a dragged element or text selection leaves a valid drop target.
22. **onDrop**: Triggered when a dragged element or text selection is dropped on a valid drop target.
23. **onCopy**: Triggered when the user copies text inside an element to the clipboard.
24. **onCut**: Triggered when the user cuts text inside an element to the clipboard.
25. **onPaste**: Triggered when the user pastes text from the clipboard into an element.


#### **States**

Whenever we want to update something based on a variable, we should be using states.
*The JavaScript comparable to states are getting an element from the DOM, and setting its text content, color, class, etc manually based on a variable or change the user has made.*

We must import useState from React whenever we'd like to use it.
`import React, { useState } from 'react';`

```
import {useState} from 'react';

function CustomButton() {
	const [count, updateCount] = useState(0)
	function handleClick() {
		updateCount(count++)
	}
	return (
		<button onClick={handleClick}>
			{count}
		</button>
	)
}
```


`const [count, updateCount] = useState(0)`
- count = the variable that will be changed
- updateCount is a function created by react that will re-render the component, **and** update our variable
- 0 is the default value for our *count* variable

`updateCount` will:
- re-render the **entire** component.
- be performed 'asynchronously'

Anything we declare within the component will be re-run on a state update. If we added a 
`let x = 0` at the top of our component, `x` will become 0 *EVERYTIME* we call the `updateCount` state despite not being related. If we needed to save `x` despite a state, we need to useState on our `x` variable as well. If we need to save `x` or any other variable to be persistent through refreshes, we need to use `cookies` to save the state, and set our default value in `useState` using that `cookie`.

If we need to useState on multiple children elements, we should put it in the nearest common parent, to avoid re-rendering and re-running unnecessary code. Using state within App is discouraged (<- this is a me thing) because it will be re-rendering the entire page.


#### **Data flow**

- Sibling components cannot pass data to each other directly
- Data can only flow up and down between parent and child
- To pass data from a child to a parent, you need the parent to pass a callback function down, that the child will then use to call up with the data as the arguments <- "Inverse data flow"

