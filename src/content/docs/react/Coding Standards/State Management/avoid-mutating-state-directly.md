---
title: Avoid Directly Mutating State
description: Avoid Directly Mutating State.
---

## Avoid Directly Mutating State

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