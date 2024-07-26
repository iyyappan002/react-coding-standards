---
title: Clean Up Side Effects
description: Clean Up Side Effects.
---


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