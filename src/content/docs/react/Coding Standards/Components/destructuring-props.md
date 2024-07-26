---
title: Destructuring Props
description: Destructuring Props.
---


## Destructuring Props

Destructure props for better readability and maintainability.


**Good**
```jsx
function UserProfile({ name, avatar }) {
  return (
    <div>
      <img src={avatar} alt="User Avatar" />
      <h1>{name}</h1>
    </div>
  );
}
```

**Bad**
```jsx
function UserProfile(props) {
  return (
    <div>
      <img src={props.avatar} alt="User Avatar" />
      <h1>{props.name}</h1>
    </div>
  );
}

```