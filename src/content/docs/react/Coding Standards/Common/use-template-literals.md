---
title: Use Template Literals
description: Use Template Literals.
---

## Use Template Literals

Use template literals to build large strings. Avoid using string concatenation. It's nice and clean.

**Bad**
```jsx
const userDetails = user.name + "'s profession is" + user.proffession

return (
  <div> {userDetails} </div>  
)
```

**Good**
```jsx
const userDetails = `${user.name}'s profession is ${user.proffession}`

return (
  <div> {userDetails} </div>  
)

```