---
title: Avoid Overusing useEffect
description: Avoid Overusing useEffect.
---


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