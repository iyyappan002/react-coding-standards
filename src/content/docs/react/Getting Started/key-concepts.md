---
title: Key Concepts of ReactJS
description: Key Concepts of ReactJS.
sidebar:
  order: 2
---


## Components

React applications are built using components. These are small, reusable pieces of code that return a React element to be rendered to the page. Components can be class-based or function-based.

## JSX 

JSX stands for JavaScript XML. It allows you to write HTML in React. JSX makes it easier to write and add HTML in React.

## State and Props

**State:**

The state is an object that represents the parts of the app that can change. Each component can have its own state.

**Props:**

Props are short for properties and allow components to receive data from their parent component.
Lifecycle Methods: These methods are available for class-based components and allow you to hook into different points in a component’s lifecycle (e.g., when the component mounts, updates, or unmounts).

## Hooks 

Hooks are functions that let you use state and other React features without writing a class. The most commonly used hooks are useState and useEffect.

## Virtual DOM: 

React uses a virtual DOM to improve performance. When the state of an object changes, React updates the virtual DOM first, then it compares the virtual DOM with the actual DOM and makes the necessary updates.