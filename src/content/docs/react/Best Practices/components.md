---
title: Components
description: Hooks.
---


## Component Size

Keep components small and focused on a single responsibility.


**Good**
```jsx
function UserProfile() {
  return (
    <div>
      <UserAvatar />
      <UserDetails />
    </div>
  );
}
```

**Bad**
```jsx
function UserProfile() {
  return (
    <div>
      <img src="user-avatar.jpg" alt="User Avatar" />
      <h1>User Name</h1>
      <p>User Description</p>
    </div>
  );
}

```

## Destructuring Props

Destructure props for better readability and maintainability.


**Good**
```jsx
function UserProfile({ name, avatar }) {
  return (
    <div>
      <img src={avatar} alt="User Avatar" />
      <h1>{name}</h1>
    </div>
  );
}
```

**Bad**
```jsx
function UserProfile(props) {
  return (
    <div>
      <img src={props.avatar} alt="User Avatar" />
      <h1>{props.name}</h1>
    </div>
  );
}

```

## Keys in Lists

Use stable keys when rendering lists.


**Good**
```jsx
function UserList({ users }) {
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

**Bad**
```jsx
function UserList({ users }) {
  return (
    <ul>
      {users.map((user, index) => (
        <li key={index}>{user.name}</li>
      ))}
    </ul>
  );
}

```

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

## Consistent Return Statements

Ensure consistent return statements in components.


**Good**
```jsx
function UserProfile({ isLoggedIn }) {
  if (!isLoggedIn) {
    return <p>Please log in.</p>;
  }

  return (
    <div>
      <UserAvatar />
      <UserDetails />
    </div>
  );
}
```

**Bad**
```jsx
function UserProfile({ isLoggedIn }) {
  if (!isLoggedIn) {
    return <p>Please log in.</p>;
  } else {
    return (
      <div>
        <UserAvatar />
        <UserDetails />
      </div>
    );
  }
}

```

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

## Do not define a function inside Render

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