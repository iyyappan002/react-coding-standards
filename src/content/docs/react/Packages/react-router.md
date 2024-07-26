---
title: React router
description: React router.
---

React Router is a popular library used for routing in React applications. It allows you to create single-page applications with navigation, enabling you to change the browser URL and render different components without refreshing the page.

:::note
To explore React Router full capabilities and features, check out the [React Router Documentation](https://reactrouter.com/)
:::

## Package overview

React Router enables "client side routing".

In traditional websites, the browser requests a document from a web server, downloads and evaluates CSS and JavaScript assets, and renders the HTML sent from the server. When the user clicks a link, it starts the process all over again for a new page.

Client side routing allows your app to update the URL from a link click without making another request for another document from the server. Instead, your app can immediately render some new UI and make data requests with fetch to update the page with new information.

This enables faster user experiences because the browser doesn't need to request an entirely new document or re-evaluate CSS and JavaScript assets for the next page. It also enables more dynamic user experiences with things like animation.


## Key Features

1. **Declarative Routing:**
    - You define routes using JSX, which makes your routes easy to understand and maintain.
2. **Nested Routing:**
    - You can create nested routes to handle complex UIs. This allows you to break down your application into smaller components, each responsible for a part of the UI.
3. **Dynamic Routing:**
    - Routes can be generated dynamically based on the application's state. This is useful for applications where routes change based on user actions or data.
4. **URL Parameters:**
    - You can define routes with parameters to handle dynamic content. For example, /user/:id can be used to show different user profiles based on the id parameter.
5. **Route Guards:**
    - You can protect routes and control access based on user authentication or other criteria.
6. **Link Component:**
    - React Router provides a <Link> component to create navigation links, which prevents full page reloads and improves the user experience.

## How to Install?

To get started with React Router, you need to install it via npm:


```cmd
npm install react-router-dom
```

This will install React Router and its dependencies, allowing you to use its components and features in your React application.

## Basic Example

Here's a simple example to demonstrate how React Router can be used:

```jsx title="App.jsx"
import { BrowserRouter as Router, Route, Switch, Link } from 'react-router-dom';

function Home() {
  return <h2>Home</h2>;
}

function About() {
  return <h2>About</h2>;
}

function Users() {
  return <h2>Users</h2>;
}

function App() {
  return (
    <Router>
      <div>
        <nav>
          <ul>
            <li>
              <Link to="/">Home</Link>
            </li>
            <li>
              <Link to="/about">About</Link>
            </li>
            <li>
              <Link to="/users">Users</Link>
            </li>
          </ul>
        </nav>

        <Switch>
          <Route path="/about">
            <About />
          </Route>
          <Route path="/users">
            <Users />
          </Route>
          <Route path="/">
            <Home />
          </Route>
        </Switch>
      </div>
    </Router>
  );
}

export default App;

```


## Advanced Features

1. **Custom Hooks:**
    - useHistory, useLocation, useParams, and useRouteMatch are hooks provided by React Router for accessing the router's state and performing navigation.
2. **Redirects:**
    - You can redirect users from one route to another programmatically using the Redirect component or history.push() method.
3. **Code Splitting:**
    - You can implement code splitting with React Router to load components lazily, improving the performance of your application.

## Conclusion
React Router is a powerful tool for building dynamic, single-page applications with React. By leveraging its features, you can create a seamless and efficient user experience with complex routing needs.