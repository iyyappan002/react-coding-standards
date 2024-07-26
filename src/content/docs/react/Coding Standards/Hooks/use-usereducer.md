---
title: Use Reducer for Complex State Logic
description: Use Reducer for Complex State Logic.
---

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