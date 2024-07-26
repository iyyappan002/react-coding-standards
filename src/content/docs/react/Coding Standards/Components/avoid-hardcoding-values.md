---
title: Avoid Hardcoding Values
description: Avoid Hardcoding Values.
---



## Avoid Hardcoding Values

Use constants or configuration files for hardcoded values.


**Good**
```jsx
const API_URL = 'https://api.example.com/users';

function fetchUsers() {
  return fetch(API_URL).then(response => response.json());
}
```

**Bad**
```jsx
function fetchUsers() {
  return fetch('https://api.example.com/users').then(response => response.json());
}

```