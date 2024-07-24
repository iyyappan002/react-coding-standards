---
title: React best practices
description: React best practices.
---

## Use Fragments

Always use Fragment over Div. It keeps the code clean and is also beneficial for performance because one less node is created in the virtual DOM.

**Bad**
```jsx
return (
  <div>
     <Component1 />
     <Component2 />
     <Component3 />
  </div>  
)
```

**Good**
```jsx
return (
  <>
     <Component1 />
     <Component2 />
     <Component3 />
  </>  
)
```

## Do not define a function inside Render

Don't define a function inside render. Try to keep the logic inside render to an absolute minimum.


**Bad**
```jsx
return (
    <button onClick={() => dispatch(ACTION_TO_SEND_DATA)}>    // NOTICE HERE
      This is a bad example 
    </button>  
)
```

**Good**
```jsx
const submitData = () => dispatch(ACTION_TO_SEND_DATA)

return (
  <button onClick={submitData}>  
    This is a good example 
  </button>  
)

```

## Use Template Literals

Use template literals to build large strings. Avoid using string concatenation. It's nice and clean.

**Bad**
```jsx
const userDetails = user.name + "'s profession is" + user.proffession

return (
  <div> {userDetails} </div>  
)
```

**Good**
```jsx
const userDetails = `${user.name}'s profession is ${user.proffession}`

return (
  <div> {userDetails} </div>  
)

```

## Component Naming

Always use PascalCase for components and camelCase for instances.

**Bad**
```jsx
import reservationCard from './ReservationCard';

const ReservationItem = <ReservationCard />;
```

**Good**
```jsx
import ReservationCard from './ReservationCard';

const reservationItem = <ReservationCard />;

```

## Props Naming

Always use camelCase for prop names or PascalCase if the prop value is a React component.

**Bad**
```jsx
<Component
  UserName="hello"
  phone_number={12345678}
/>
```

**Good**
```jsx
<MyComponent
  userName="hello"
  phoneNumber={12345678}
  Component={SomeComponent}
/>

```

## Function arguments (2 or fewer ideally)

**Bad**
```jsx
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);
```

**Good**
```jsx
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true
});

```

## Local State for Local Data

Use local state (using useState) for components that don't need to share data with others.

**Good**
```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

**Bad**
```jsx
import React, { useContext, useState } from 'react';

const GlobalStateContext = React.createContext();

function Counter() {
  const { count, setCount } = useContext(GlobalStateContext);

  return (
    <div>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

function App() {
  const [count, setCount] = useState(0);

  return (
    <GlobalStateContext.Provider value={{ count, setCount }}>
      <Counter />
    </GlobalStateContext.Provider>
  );
}

```

## Use Reducer for Complex State Logic

Use useReducer for complex state logic or when the state has multiple sub-values.

**Bad**
```jsx
import React, { useState } from 'react';

function App() {
  const [todos, setTodos] = useState([]);
  const [input, setInput] = useState('');

  const addTodo = () => {
    setTodos([...todos, { id: Date.now(), text: input, completed: false }]);
    setInput('');
  };

  const toggleTodo = id => {
    setTodos(
      todos.map(todo =>
        todo.id === id ? { ...todo, completed: !todo.completed } : todo
      )
    );
  };

  const deleteTodo = id => {
    setTodos(todos.filter(todo => todo.id !== id));
  };

  return (
    <div>
      <input value={input} onChange={e => setInput(e.target.value)} />
      <button onClick={addTodo}>Add To-Do</button>
      <ul>
        {todos.map(todo => (
          <li key={todo.id}>
            <span
              onClick={() => toggleTodo(todo.id)}
              style={{ textDecoration: todo.completed ? 'line-through' : 'none' }}
            >
              {todo.text}
            </span>
            <button onClick={() => deleteTodo(todo.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default App;

```

Explanation:

- **State Management**: Multiple <font color="green">useState</font> calls to manage the to-do list and input state.
- **Action Handlers**: Three separate functions (<font color="green">addTodo</font>, <font color="green">toggleTodo</font>, <font color="green">deleteTodo</font>) to handle different actions.
- **Code Complexity**: As the number of actions and state variables increases, managing state with <font color="green">useState</font> becomes more complex and harder to maintain.

**Good**
```jsx
import React, { useReducer, useState } from 'react';

const initialState = [];

function reducer(state, action) {
  switch (action.type) {
    case 'add':
      return [...state, { id: Date.now(), text: action.payload, completed: false }];
    case 'toggle':
      return state.map(todo =>
        todo.id === action.payload ? { ...todo, completed: !todo.completed } : todo
      );
    case 'delete':
      return state.filter(todo => todo.id !== action.payload);
    default:
      throw new Error('Unknown action type');
  }
}

function App() {
  const [state, dispatch] = useReducer(reducer, initialState);
  const [input, setInput] = useState('');

  const addTodo = () => {
    dispatch({ type: 'add', payload: input });
    setInput('');
  };

  return (
    <div>
      <input value={input} onChange={e => setInput(e.target.value)} />
      <button onClick={addTodo}>Add To-Do</button>
      <ul>
        {state.map(todo => (
          <li key={todo.id}>
            <span
              onClick={() => dispatch({ type: 'toggle', payload: todo.id })}
              style={{ textDecoration: todo.completed ? 'line-through' : 'none' }}
            >
              {todo.text}
            </span>
            <button onClick={() => dispatch({ type: 'delete', payload: todo.id })}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default App;

```


Explanation:

- **Initial State**: An empty array to store to-do items.
- **Reducer**: Handles three actions:
  - <font color="green">add</font>: Adds a new to-do item to the state.
  - <font color="green">toggle</font>: Toggles the completion status of a to-do item.
  - <font color="green">delete</font>: Removes a to-do item from the state.
- **App Component**: 
  - Manages the input state for the new to-do text.
  - Dispatches actions to the reducer to modify the to-do list.
  - Renders the list of to-dos with options to toggle and delete.

## Specify Dependencies Correctly:

Ensure the dependency array includes all variables used within useEffect.Omitting dependencies or use incorrect dependencies, leading to unexpected behavior.

**Good**
```jsx
import React, { useState, useEffect } from 'react';

function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds(prevSeconds => prevSeconds + 1);
    }, 1000);
    return () => clearInterval(interval);
  }, []);

  return <div>Seconds: {seconds}</div>;
}

export default Timer;
```

**Bad**
```jsx
import React, { useState, useEffect } from 'react';

function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds(seconds + 1);
    }, 1000);
    return () => clearInterval(interval);
  }, []); // Missing seconds dependency

  return <div>Seconds: {seconds}</div>;
}

export default Timer;

```

## Clean Up Side Effects

Clean up side effects such as subscriptions or timers in the return function of useEffect.Don’t Forget to clean up side effects, leading to memory leaks.

**Good**
```jsx
import React, { useState, useEffect } from 'react';

function WindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);

    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return <div>Width: {width}px</div>;
}

export default WindowWidth;
```

**Bad**
```jsx
import React, { useState, useEffect } from 'react';

function WindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);

    window.addEventListener('resize', handleResize);
    // Missing cleanup function
  }, []);

  return <div>Width: {width}px</div>;
}

export default WindowWidth;

```


## Avoid Overusing useEffect

Use useEffect for side effects only. For pure logic based on props or state, prefer memoization techniques or conditional rendering.Use useEffect to handle logic that can be done elsewhere.

**Good**
```jsx
import React, { useState } from 'react';

function FilteredList({ items, filter }) {
  const filteredItems = items.filter(item => item.includes(filter));
  
  return (
    <ul>
      {filteredItems.map(item => (
        <li key={item}>{item}</li>
      ))}
    </ul>
  );
}

export default FilteredList;
```

**Bad**
```jsx
import React, { useState, useEffect } from 'react';

function FilteredList({ items, filter }) {
  const [filteredItems, setFilteredItems] = useState([]);

  useEffect(() => {
    setFilteredItems(items.filter(item => item.includes(filter)));
  }, [items, filter]);

  return (
    <ul>
      {filteredItems.map(item => (
        <li key={item}>{item}</li>
      ))}
    </ul>
  );
}

export default FilteredList;

```

## Use Functional Updates When Needed

Use functional updates when the new state depends on the previous state. Don’t Directly use the state variable without considering its asynchronous nature.

**Good**
```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(prevCount => prevCount + 1);
  };

  return (
    <div>
      <p>{count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}

export default Counter;
```

**Bad**
```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1); // This can cause issues if count is not up-to-date
  };

  return (
    <div>
      <p>{count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}

export default Counter;

```

## Avoid Unnecessary State

Only use state for values that change over time and need to trigger re-renders.
Don't Store static or derived data in state unnecessarily


**Good**
```jsx
import React, { useState } from 'react';

function NameList({ names }) {
  return (
    <ul>
      {names.map(name => (
        <li key={name}>{name}</li>
      ))}
    </ul>
  );
}

export default NameList;
```

**Bad**
```jsx
import React, { useState, useEffect } from 'react';

function NameList({ names }) {
  const [nameList, setNameList] = useState(names); // Unnecessary state

  useEffect(() => {
    setNameList(names);
  }, [names]);

  return (
    <ul>
      {nameList.map(name => (
        <li key={name}>{name}</li>
      ))}
    </ul>
  );
}

export default NameList;

```

## Component Size

Keep components small and focused on a single responsibility.


**Good**
```jsx
function UserProfile() {
  return (
    <div>
      <UserAvatar />
      <UserDetails />
    </div>
  );
}
```

**Bad**
```jsx
function UserProfile() {
  return (
    <div>
      <img src="user-avatar.jpg" alt="User Avatar" />
      <h1>User Name</h1>
      <p>User Description</p>
    </div>
  );
}

```

## Destructuring Props

Destructure props for better readability and maintainability.


**Good**
```jsx
function UserProfile({ name, avatar }) {
  return (
    <div>
      <img src={avatar} alt="User Avatar" />
      <h1>{name}</h1>
    </div>
  );
}
```

**Bad**
```jsx
function UserProfile(props) {
  return (
    <div>
      <img src={props.avatar} alt="User Avatar" />
      <h1>{props.name}</h1>
    </div>
  );
}

```

## Keys in Lists

Use stable keys when rendering lists.


**Good**
```jsx
function UserList({ users }) {
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

**Bad**
```jsx
function UserList({ users }) {
  return (
    <ul>
      {users.map((user, index) => (
        <li key={index}>{user.name}</li>
      ))}
    </ul>
  );
}

```

## Avoid Hardcoding Values

Use constants or configuration files for hardcoded values.


**Good**
```jsx
const API_URL = 'https://api.example.com/users';

function fetchUsers() {
  return fetch(API_URL).then(response => response.json());
}
```

**Bad**
```jsx
function fetchUsers() {
  return fetch('https://api.example.com/users').then(response => response.json());
}

```

## Consistent Return Statements

Ensure consistent return statements in components.


**Good**
```jsx
function UserProfile({ isLoggedIn }) {
  if (!isLoggedIn) {
    return <p>Please log in.</p>;
  }

  return (
    <div>
      <UserAvatar />
      <UserDetails />
    </div>
  );
}
```

**Bad**
```jsx
function UserProfile({ isLoggedIn }) {
  if (!isLoggedIn) {
    return <p>Please log in.</p>;
  } else {
    return (
      <div>
        <UserAvatar />
        <UserDetails />
      </div>
    );
  }
}

```

## Avoid Excessive Nesting

Avoid unnecessary nesting of elements.


**Good**
```jsx
function Card() {
  return (
    <div className="bg-white shadow-md rounded p-4">
      <h2 className="text-xl font-bold">Card Title</h2>
      <p className="text-gray-700">Card description...</p>
    </div>
  );
}
```

**Bad**
```jsx
function Card() {
  return (
    <div className="bg-white">
      <div className="shadow-md">
        <div className="rounded">
          <div className="p-4">
            <h2 className="text-xl font-bold">Card Title</h2>
            <p className="text-gray-700">Card description...</p>
          </div>
        </div>
      </div>
    </div>
  );
}

```

## Extract Components

Extract reusable components to avoid duplication.


**Good**
```jsx
function Button({ children }) {
  return <button className="bg-blue-500 text-white px-4 py-2 rounded">{children}</button>;
}

function App() {
  return (
    <div>
      <Button>Click Me</Button>
      <Button>Submit</Button>
    </div>
  );
}
```

**Bad**
```jsx
function App() {
  return (
    <div>
      <button className="bg-blue-500 text-white px-4 py-2 rounded">Click Me</button>
      <button className="bg-blue-500 text-white px-4 py-2 rounded">Submit</button>
    </div>
  );
}

```

## Use Link Instead of \<a> for Navigation

Use Link from react-router-dom for navigation to prevent full page reloads.


**Good**
```jsx
import { Link } from 'react-router-dom';

function Navbar() {
  return (
    <nav>
      <Link to="/">Home</Link>
      <Link to="/about">About</Link>
    </nav>
  );
}
```

**Bad**
```jsx
function Navbar() {
  return (
    <nav>
      <a href="/">Home</a>
      <a href="/about">About</a>
    </nav>
  );
}

```

## Nested Routes

Use nested routes for better organization.


**Good**
```jsx
import { Route, Switch, useRouteMatch } from 'react-router-dom';

function Dashboard() {
  let { path, url } = useRouteMatch();
  return (
    <div>
      <nav>
        <Link to={`${url}/profile`}>Profile</Link>
        <Link to={`${url}/settings`}>Settings</Link>
      </nav>
      <Switch>
        <Route path={`${path}/profile`} component={Profile} />
        <Route path={`${path}/settings`} component={Settings} />
      </Switch>
    </div>
  );
}
```

**Bad**
```jsx
import { Route, Switch } from 'react-router-dom';

function Dashboard() {
  return (
    <div>
      <nav>
        <Link to="/dashboard/profile">Profile</Link>
        <Link to="/dashboard/settings">Settings</Link>
      </nav>
      <Switch>
        <Route path="/dashboard/profile" component={Profile} />
        <Route path="/dashboard/settings" component={Settings} />
      </Switch>
    </div>
  );
}

```

## Lazy Loading with React.lazy


:::note
Use React.lazy and Suspense for code splitting.


lazy loading is a technique used to improve the performance of a React application by loading components only when they are needed. Instead of loading all components at once, lazy loading ensures that only the components required for the initial view are loaded first, while other components are loaded as the user navigates through the app. This can lead to faster initial load times and a better user experience, especially in large applications.

:::






**Good**
```jsx
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import React, { Suspense, lazy } from 'react';

const Home = lazy(() => import('./Home'));
const About = lazy(() => import('./About'));

function App() {
  return (
    <Router>
      <Suspense fallback={<div>Loading...</div>}>
        <Switch>
          <Route path="/" exact component={Home} />
          <Route path="/about" component={About} />
        </Switch>
      </Suspense>
    </Router>
  );
}
```

**Bad**
```jsx
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import Home from './Home';
import About from './About';

function App() {
  return (
    <Router>
      <Switch>
        <Route path="/" exact component={Home} />
        <Route path="/about" component={About} />
      </Switch>
    </Router>
  );
}

```

## Use useMemo Hook


:::note
Use useMemo to memoize expensive calculations.

useMemo is a React hook that memoizes the result of an expensive calculation, recomputing it only when its dependencies change. This helps optimize performance by avoiding unnecessary recalculations on every render. It's particularly useful for computationally intensive operations or maintaining referential equality of objects and arrays, thereby preventing unnecessary re-renders of child components. Use it to ensure your React applications run efficiently, but only when you identify performance bottlenecks to avoid premature optimization.

:::


**Good**
```jsx
import React, { useState, useMemo } from 'react';

function ParentComponent() {
  const [count, setCount] = useState(0);
  const expensiveValue = useMemo(() => {
    console.log('Calculating expensive value');
    return count * 2;
  }, [count]);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <div>Expensive Value: {expensiveValue}</div>
    </div>
  );
}
```

**Bad**
```jsx
function ParentComponent() {
  const [count, setCount] = useState(0);
  const expensiveValue = (() => {
    console.log('Calculating expensive value');
    return count * 2;
  })();

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <div>Expensive Value: {expensiveValue}</div>
    </div>
  );
}

```