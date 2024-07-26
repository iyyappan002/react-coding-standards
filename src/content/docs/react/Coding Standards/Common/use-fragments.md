---
title: Use Fragments
description: Use Fragments.
---



## Use Fragments

Always use Fragment over Div. It keeps the code clean and is also beneficial for performance because one less node is created in the virtual DOM.

**Bad**
```jsx
return (
  <div>
     <Component1 />
     <Component2 />
     <Component3 />
  </div>  
)
```

**Good**
```jsx
return (
  <>
     <Component1 />
     <Component2 />
     <Component3 />
  </>  
)
```