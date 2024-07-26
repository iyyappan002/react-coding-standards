---
title: Avoid Unnecessary State
description: Avoid Unnecessary State.
---

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
