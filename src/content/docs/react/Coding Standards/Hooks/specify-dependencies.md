---
title: Specify Dependencies Correctly
description: Specify Dependencies Correctly.
---

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