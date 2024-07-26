---
title: Avoid Excessive Nesting
description: Avoid Excessive Nesting.
---


## Avoid Excessive Nesting

Avoid unnecessary nesting of elements.


**Good**
```jsx
function Card() {
  return (
    <div className="bg-white shadow-md rounded p-4">
      <h2 className="text-xl font-bold">Card Title</h2>
      <p className="text-gray-700">Card description...</p>
    </div>
  );
}
```

**Bad**
```jsx
function Card() {
  return (
    <div className="bg-white">
      <div className="shadow-md">
        <div className="rounded">
          <div className="p-4">
            <h2 className="text-xl font-bold">Card Title</h2>
            <p className="text-gray-700">Card description...</p>
          </div>
        </div>
      </div>
    </div>
  );
}

```