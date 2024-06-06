Routes are stackable. You can put a route inside a route.
Example:
Put /users to display the users page, but add a /users/*ID* displays a specific profile

`npm install react-router-dom@6.22.3`

```
// App.js
import {BrowserRouter, Routes, Route} from 'react-router-dom'

function App() {
	return (
		<BrowserRouter>
			<Navbar />
			<Routes>
				<Route path="/user" element={<User userData={userData}/>}/>
				<Route path="/projects" element={<Projects/>}/>
				<Route path="*" element={<div>404 NOT FOUND</div>}/>
			</Routes>
		</BrowserRouter>
	)
}


// Navbar.js
import {Link} from 'react-router-dom'

function Navbar() {
	return (
		<Link to="/">Home</Link>
		<Link to="/user">User</Link>
		<Link to="/projects">Projects</Link>
	)
}
export default Navbar
```

*App.js*
**Path** is the extension of the route. https://j-gray.cc/projects, the path would be /projects

**Element** is the component you'd like to render at that path. You can pass props directly into your element components as usual. (See `<User/>` component, passing `userData`). You can only put one component in the element, however that element component may have children as usual.

*Navbar.js*
Our Navbar will **ALWAYS** be loaded. It will display regardless of what our current route is. This is because of where we placed it in our code, within our `<BrowserRouter>`, but outside our `<Routes>`

Link is how we should we redirecting with a router. using `a href` will still work and redirect as intended, however using link will redirect *without* reloading the page


Setting the Path to "/" is the "default" route. It will display on first page load as the "home" page.
Setting the Path to "\*" will become the "Not found" page. It catches any undefined routes, and displays whatever element you'd like (For example a custom 404 page)

We could *(not shown in example)* use ternary's to display our Routes. Useful for user login.
The route completely doesn't exist unless the ternary displays it. So check if the user is logged in, then create the route to the "protected" content


### useNavigate

```
import {useNavigate} from 'react-router-dom'
```

We must set a const to useNavigate(), or react gets unhappy
```
const navigate = useNavigate()`
```

Using **useNavigate**, we can add a `navigate('/')` anywhere in our code (Specifically after an event, login, etc) to redirect in this case to our home "/" route. `navigate('/user')` 

We can also call `navigate()` directly through a `onClick` event
```
<button onClick={()=>navigate('/route')}>Navigate using useNavigate</button>
```

**Using navigate, we maintain our state because we are not refreshing the page.**

Putting it all together:
```
import {useNavigate} from 'react-router-dom'

function button() {
	const navigate = useNavigate()
	return (
		<button onClick={()=>navigate('/user')}>Click to view user route</button>
	)
}
```





In order to nest routes (/projects/projectid),
we set our App route to "/route/\*", and in our component, we return routes


```
//App.jsx

import "../styles/App.css";
import { BrowserRouter, Routes, Route } from "react-router-dom";
import User from "./User.jsx";

const App = () => {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<div>Home page</div>} />
        <Route path="*" element={<div>BASE ROUTE NOT FOUND</div>} />
        <Route path="/user/*" element={<User />} />
      </Routes>
    </BrowserRouter>
  );
};

export default App;
```


```
//User.jsx

import { Route, Routes } from "react-router-dom";

function User() {
  return (
    <Routes>
      <Route path="/" element={<div>DEFAULT USER PAGE</div>} />
      <Route path="nested" element={<div>NESTED USER PAGE</div>} />
      <Route path="*" element={<div>NESTED PAGE NOT FOUND</div>} />
    </Routes>
  );
}

export default User;
```