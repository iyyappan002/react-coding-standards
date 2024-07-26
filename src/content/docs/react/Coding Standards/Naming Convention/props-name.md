---
title: Props Naming
description: Props Naming.
---


## Props Naming

Always use camelCase for prop names or PascalCase if the prop value is a React component.

**Bad**
```jsx
<Component
  UserName="hello"
  phone_number={12345678}
/>
```

**Good**
```jsx
<MyComponent
  userName="hello"
  phoneNumber={12345678}
  Component={SomeComponent}
/>

```
