---
title: Use Functional Updates When Needed
description: Use Functional Updates When Needed.
---


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