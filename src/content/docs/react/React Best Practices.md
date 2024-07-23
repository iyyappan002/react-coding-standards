---
title: React best practices
description: React best practices.
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

## Do not define a function inside Render

Don't define a function inside render. Try to keep the logic inside render to an absolute minimum.


**Bad**
```jsx
return (
    <button onClick={() => dispatch(ACTION_TO_SEND_DATA)}>    // NOTICE HERE
      This is a bad example 
    </button>  
)
```

**Good**
```jsx
const submitData = () => dispatch(ACTION_TO_SEND_DATA)

return (
  <button onClick={submitData}>  
    This is a good example 
  </button>  
)

```