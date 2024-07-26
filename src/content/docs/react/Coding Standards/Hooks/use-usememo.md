---
title: Use useMemo Hook
description: Use useMemo Hook.
---


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
