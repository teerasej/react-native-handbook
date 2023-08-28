

# Setup Redux store ให้ App component


```jsx
// src/App.js 

import { StatusBar } from 'expo-status-bar';
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';
import CounterNumber from './components/CounterNumber';
import IncreaseButton from './components/IncreaseButton';

// Provider component จาก redux 
import { Provider } from "react-redux";

// import function มาจาก store.js
import configureStore from "./redux/store";

// สร้าง object ของ store
const store = configureStore();

function App() {
  return (
    {/* กำหนด store object ให้ Provider ที่ครอบ component ทุกตัว */}
    <Provider store={store}>
      <View style={styles.container}>
        <CounterNumber />
        <IncreaseButton />
        <StatusBar style="auto" />
      </View>
    </Provider>
  );
}

export default App;
```