---
title: Naming
description: Naming.
---


## Component Naming

Always use PascalCase for components and camelCase for instances.

**Bad**
```jsx
import reservationCard from './ReservationCard';

const ReservationItem = <ReservationCard />;
```

**Good**
```jsx
import ReservationCard from './ReservationCard';

const reservationItem = <ReservationCard />;

```

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
