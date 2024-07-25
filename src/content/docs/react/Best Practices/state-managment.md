---
title: State Management
description: State Management.
---

## Avoid Directly Mutating State in React

Use methods like spread operator or concat to create a new array or object when updating state. Don't directly changing the state variable (e.g., using push or direct assignment) leads to unpredictable behavior and bugs.

**Good**
```jsx
const [todos, setTodos] = useState([{ text: 'Learn React' }]);

const addTodo = () => {
  setTodos([...todos, { text: 'Learn more about hooks' }]);
};
```

**Bad**
```jsx
const [todos, setTodos] = useState([{ text: 'Learn React' }]);

const addTodo = () => {
  todos.push({ text: 'Learn more about hooks' });
  setTodos(todos);
};
```

## Using Reducers for Complex State Logic

When managing complex state logic in React, using reducers can help you organize and manage state updates more effectively. Reducers are functions that take the current state and an action, then return a new state based on the action type.

**Why Use Reducers?**

- **Predictable State Transitions:** Reducers ensure that state transitions are predictable and centralized.
- **Maintainability:** Complex state logic is easier to manage, debug, and maintain.
- **Testability:** Reducer functions can be easily tested in isolation.

**How to Use Reducers:**

1. Define Initial State and Reducer Function:

```jsx
const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      throw new Error('Unknown action type');
  }
}

```

2. Use useReducer Hook in Your Component:

```jsx
import React, { useReducer } from 'react';

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </div>
  );
}

```

**Example Explanation:**

1. **Reducer Function:**
    - Takes the current state and an action.
    - Uses a switch statement to handle different action types.
    - Returns a new state based on the action.
2. **useReducer Hook:**
    - Initializes the state with initialState.
    - Provides state and dispatch to handle state updates.
    - dispatch is used to send actions to the reducer.