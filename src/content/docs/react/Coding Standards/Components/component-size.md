---
title: Component Size
description: Component Size.
---


## Component Size

Keep components small and focused on a single responsibility.


**Good**
```jsx
function UserProfile() {
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
function UserProfile() {
  return (
    <div>
      <img src="user-avatar.jpg" alt="User Avatar" />
      <h1>User Name</h1>
      <p>User Description</p>
    </div>
  );
}

```