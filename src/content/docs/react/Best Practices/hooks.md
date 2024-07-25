---
title: Hooks
description: Hooks.
---

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


## Use useMemo Hook

Use useMemo to memoize expensive calculations.

:::note
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
