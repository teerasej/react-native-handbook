
# React Native Profiler


## 1. Set up a new React Native project
Create a new React Native project with expo using the following command:

```bash
npx create-expo-app ProfilingExample
```
Navigate to the project directory:

```bash
cd ProfilingExample
```

## 2. Create a simple React component

This component will be used to demonstrate the effect of useCallback and useEffect. It should have a prop that changes over time, causing the component to re-render.

```jsx
import React, { useState, useEffect } from 'react';
import { Button, Text, View } from 'react-native';

export default function Counter() {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);
  };

  useEffect(() => {
    console.log('Component re-rendered!');
  });

  return (
    <View>
      <Text>{count}</Text>
      <Button title="Increment" onPress={increment} />
    </View>
  );
}
```

In the above code, the increment function is recreated every time the component re-renders, causing unnecessary re-renders of the Button component. Also, the useEffect hook runs on every render, even when the count state doesn't change.

## 3. Add the Counter component to App.js

Open the `App.js` file and import the Counter component.

```jsx
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View } from 'react-native';
import Counter from './Counter';

export default function App() {
  return (
    <View style={styles.container}>
      <Text>Open up App.js to start working on your app!</Text>
      <Counter />
      <StatusBar style="auto" />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
});

```

## 4. Run the App


Run the application using the following command:

```bash
npm run android
npm run ios
npm run web
```

### noted for running on web

```
npx expo install react-native-web react-dom @expo/metro-runtime
```

## 5. Observe the console

Every time you click the "Increment" button, you should see "Component re-rendered!" in the console. This is because the Counter component is re-rendering every time its state changes.   

## 6. Optimizing with useCallback and useEffect

To prevent the unnecessary re-renders of the Button component, we can use the useCallback hook to memoize the increment function. We can also pass a dependency array to the useEffect hook to control when it runs.

```jsx
import React, { useState, useEffect, useCallback } from 'react';
import { Button, Text, View } from 'react-native';

export default function Counter() {
  const [count, setCount] = useState(0);

  const increment = useCallback(() => {
    setCount(c => c + 1);
  }, []);

  useEffect(() => {
    console.log('Count state changed, component re-rendered!');
  }, [count]);

  return (
    <View>
      <Text>{count}</Text>
      <Button title="Increment" onPress={increment} />
    </View>
  );
}
```