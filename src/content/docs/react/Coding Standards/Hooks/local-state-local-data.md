---
title: Local State for Local Data
description: Local State for Local Data.
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