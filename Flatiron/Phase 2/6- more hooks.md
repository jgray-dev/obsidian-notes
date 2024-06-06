useRef - Saves everything as an object, and maintains similar to state - but doesn't refresh the page.

`const counter = useRef(10)`



useContext - Allows code to be used anywhere in your project. Setup a new file as follows:

```
//UserContext.js

import {createContext} from 'react'
export default UserContext = createContext(undefined)
```

Wrapping another component in a
```
<UserContext.provider value={state/variable/whatever}>
</UserContext.provider>
```
will allow us to access the data stored within our UserContext.js. When we wrap our code in the above, we need to set value to a "default" value (state/variable/whatever).

