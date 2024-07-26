---
title: Consistent Return Statements
description: Consistent Return Statements.
---


## Consistent Return Statements

Ensure consistent return statements in components.


**Good**
```jsx
function UserProfile({ isLoggedIn }) {
  if (!isLoggedIn) {
    return <p>Please log in.</p>;
  }

  return (
    <div>
      <UserAvatar />
      <UserDetails />
    </div>
  );
}
```

**Bad**
```jsx
function UserProfile({ isLoggedIn }) {
  if (!isLoggedIn) {
    return <p>Please log in.</p>;
  } else {
    return (
      <div>
        <UserAvatar />
        <UserDetails />
      </div>
    );
  }
}

```