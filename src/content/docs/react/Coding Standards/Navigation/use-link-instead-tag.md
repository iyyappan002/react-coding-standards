---
title: Use Link Instead of \<a> for Navigation
description: Use Link Instead of \<a> for Navigation.
---

## Use Link Instead of \<a> for Navigation

Use Link from react-router-dom for navigation to prevent full page reloads.


**Good**
```jsx
import { Link } from 'react-router-dom';

function Navbar() {
  return (
    <nav>
      <Link to="/">Home</Link>
      <Link to="/about">About</Link>
    </nav>
  );
}
```

**Bad**
```jsx
function Navbar() {
  return (
    <nav>
      <a href="/">Home</a>
      <a href="/about">About</a>
    </nav>
  );
}

```