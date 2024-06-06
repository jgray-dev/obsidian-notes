
Our way of telling react "When *x* loads, do *y*"

Like using a fetch, loading external data, etc, we might want to display a loading icon, or loading message in the meantime while we load the data. 
It will call a useEffect function when the page first loads. 
- If the dependencies array does not exist, it will re-fire on *EVERY* page load/render. 
- If the dependencies array is set, but empty, it will only fire *once* when the page loads. 
- If the dependency array is set with states as arguments, it will fire *EVERY TIME* that state is updated.


useEffect mostly used for fetching. Using it for only the dependency array doesn't make sense, you should just useState if that's what you'd like to do.
```
useEffect(()=>{
	const URL = 'http://localhost:4000/projects'
	fetch(`${URL}`)
	.then(r=>r.json())
	.then(data=>{
		setProjectState(data)
	})
},[])
```
The empty array at the end is called the "Dependency array"
The dependency array is sort of like state, listening for changes of state of variables within the array, and will re-fire the useEffect block when a change is detected

```
import { useState, useEffect } from "react";

function Countdown() {
  const [count, updateCounter] = useState(0);
  useEffect(() => {
    console.log("Counter updated (firing useEffect): ", count);
  }, [count]);

  function handleClick() {
    updateCounter(count + 1);
  }
  return <button onClick={handleClick}>{count}</button>;
}

export default Countdown;
```


Example of using the dependency array to track changes to our `counter` state, re-firing our useEffect every time counter is updated.


```
import { useState, useEffect } from "react";

function Mousetrack() {
  const [mousePos, setMousePos] = useState([]);

  useEffect(() => {
    console.log("useEffect called. MousePos: ", mousePos);

    function handleEvent(event) {
      setMousePos({ x: event.clientX, y: event.clientY });
    }

    window.addEventListener("mousemove", handleEvent);
    return () => {
      window.removeEventListener("mousemove", handleEvent);
    };
  }, [mousePos]);
  return <></>;
}

export default Mousetrack;
```

This is an example of a "cleanup". using the `return(()=>{})` function within `useEffect`, and moving the event listener to a function rather than in-line, we can remove the event listener when the page unloads. (`return` is fired whenever the page unloads)