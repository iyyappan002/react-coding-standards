---
title: Component Naming
description: Component Naming.
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