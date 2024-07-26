---
title: Extract Components
description: Extract Components.
---



## Extract Components

Extract reusable components to avoid duplication.


**Good**
```jsx
function Button({ children }) {
  return <button className="bg-blue-500 text-white px-4 py-2 rounded">{children}</button>;
}

function App() {
  return (
    <div>
      <Button>Click Me</Button>
      <Button>Submit</Button>
    </div>
  );
}
```

**Bad**
```jsx
function App() {
  return (
    <div>
      <button className="bg-blue-500 text-white px-4 py-2 rounded">Click Me</button>
      <button className="bg-blue-500 text-white px-4 py-2 rounded">Submit</button>
    </div>
  );
}

```