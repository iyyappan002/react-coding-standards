---
title: Navigation
description: Navigation.
---

## Use Link Instead of \<a> for Navigation

Use Link from react-router-dom for navigation to prevent full page reloads.


**Good**
```jsx
import { Link } from 'react-router-dom';

function Navbar() {
  return (
    <nav>
      <Link to="/">Home</Link>
      <Link to="/about">About</Link>
    </nav>
  );
}
```

**Bad**
```jsx
function Navbar() {
  return (
    <nav>
      <a href="/">Home</a>
      <a href="/about">About</a>
    </nav>
  );
}

```

## Nested Routes

Use nested routes for better organization.


**Good**
```jsx
import { Route, Switch, useRouteMatch } from 'react-router-dom';

function Dashboard() {
  let { path, url } = useRouteMatch();
  return (
    <div>
      <nav>
        <Link to={`${url}/profile`}>Profile</Link>
        <Link to={`${url}/settings`}>Settings</Link>
      </nav>
      <Switch>
        <Route path={`${path}/profile`} component={Profile} />
        <Route path={`${path}/settings`} component={Settings} />
      </Switch>
    </div>
  );
}
```

**Bad**
```jsx
import { Route, Switch } from 'react-router-dom';

function Dashboard() {
  return (
    <div>
      <nav>
        <Link to="/dashboard/profile">Profile</Link>
        <Link to="/dashboard/settings">Settings</Link>
      </nav>
      <Switch>
        <Route path="/dashboard/profile" component={Profile} />
        <Route path="/dashboard/settings" component={Settings} />
      </Switch>
    </div>
  );
}

```
