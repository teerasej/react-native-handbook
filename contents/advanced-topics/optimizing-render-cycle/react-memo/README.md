
# Testing the React.memo()

> Test at https://snack.expo.dev/ 

Sure, here's a step-by-step guide on how to prove that React.memo can prevent unnecessary re-rendering and improve performance:

## 0. Set up a new React Native project
Create a new React Native project with expo using the following command:

```bash
npx create-expo-app ReactMemoExample
```
Navigate to the project directory:

```bash
cd ReactMemoExample
```

## 1. Create a simple React component

This component will be used to demonstrate the effect of React.memo. It should have a prop that changes over time, causing the component to re-render.

```jsx
function MyComponent({ count }) {
  console.log('MyComponent rendered');
  return <div>{count}</div>;
}
```

## 2. Create a parent component

This component will contain MyComponent and a state variable that changes over time, causing MyComponent to re-render.

```jsx
import { Text, View, SafeAreaView, StyleSheet } from 'react-native';
import { useState } from "react";

function ParentComponent() {
  const [count, setCount] = React.useState(0);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Increase Count</button>
      <MyComponent count={count} />
    </div>
  );
}
```

## 3. Observe the console

Every time you click the "Increase Count" button, you should see "MyComponent rendered" in the console. This is because MyComponent is re-rendering every time its props change.

## 4. Add React.memo to MyComponent

This will prevent MyComponent from re-rendering unless its props change.

```jsx
const MyComponent = React.memo(function MyComponent({ count }) {
  console.log('MyComponent rendered');
  return <div>{count}</div>;
});
```

## 5. Observe the console again

Now, when you click the "Increase Count" button, you should see "MyComponent rendered" in the console only when the count actually changes. This demonstrates that React.memo is preventing unnecessary re-renders and potentially improving performance.
