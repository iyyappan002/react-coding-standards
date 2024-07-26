---
title: Nested Routes
description: Nested Routes.
---


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