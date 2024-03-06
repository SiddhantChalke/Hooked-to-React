# Hooked-to-React
## A guide to use hooks in react functional components

React hooks are functions that enable functional components to manage state and side effects. They were introduced in React 16.8 and have become an integral part of building React applications. Here, we'll explore the basic use cases of some of the commonly used React hooks.

## 1. useState

`useState` is used for managing state in functional components. It returns an array with two elements: the current state value and a function to update it. Here's a basic example:

```jsx
import React, { useState } from 'react';

function Counter() {
    const [count, setCount] = useState(0);

  return (
      <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

## 2. useEffect
`useEffect` is used for handling side effects in functional components, such as data fetching, subscriptions, or manual DOM manipulations. Here is an example fetching data:

```jsx
import React, { useState, useEffect } from 'react';

function DataFetcher() {
  const [data, setData] = useState([]);

  useEffect(() => {
    // Fetch data from an API
    fetch('https://api.example.com/data')
      .then(response => response.json())
      .then(data => setData(data))
      .catch(error => console.error('Error fetching data:', error));
  }, []); // Empty dependency array means it runs once after initial render

  return (
    <div>
      <ul>
        {data.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

## 3. useContext
`useContext` is used for consuming values from the React context. It allows components to subscribe to a context without introducing nesting.

```jsx
import React, { createContext, useContext } from 'react';

const ThemeContext = createContext('light');

function ThemedComponent() {
  const theme = useContext(ThemeContext);

  return <div style={{ background: theme }}>Themed Content</div>;
}
```

## 4. useReducer
`useReducer` is used for managing more complex state logic. It is similar to `useState` but allows you to handle state transitions in a more predictable way.

```jsx
import React, { useReducer } from 'react';

const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      throw new Error();
  }
}

function CounterWithReducer() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>Decrement</button>
    </div>
  );
}
```

## 5. useCallback
`useCallback` is used to memoize functions, preventing unnecessary re-creation of functions in every render. It's particularly useful when passing functions as props to child components, preventing unnecessary renders of those child components.

In this example, `handleIncrement` is memoized with `useCallback`, ensuring that it remains the same between renders as long as the `count` state remains unchanged.
```jsx
import React, { useState, useCallback } from 'react';

function ParentComponent() {
  const [count, setCount] = useState(0);

  const handleIncrement = useCallback(() => {
    setCount(count + 1);
  }, [count]);

  return (
    <div>
      <p>Count: {count}</p>
      <ChildComponent onIncrement={handleIncrement} />
    </div>
  );
}

function ChildComponent({ onIncrement }) {
  return <button onClick={onIncrement}>Increment</button>;
}
```

## 6. useMemo
`useMemo` is used to memoize the result of a computation, preventing redundant calculations during renders. It's helpful when dealing with expensive calculations or complex data transformations.

In this example, the result of the expensive calculation is memoized with `useMemo`, so it only recalculates when the `data` dependency changes.

```jsx
import React, { useMemo } from 'react';

function ExpensiveCalculationComponent({ data }) {
  const result = useMemo(() => {
    // Expensive calculation based on data
    return data.reduce((acc, value) => acc + value, 0);
  }, [data]);

  return <p>Result: {result}</p>;
}

```