---
title: Avoid function definition inside Render
description: Avoid function definition inside Render.
---


## Avoid function definition inside Render

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