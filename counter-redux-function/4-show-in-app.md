
# แสดงทั้ง 2 component ไว้ใน App component


```jsx
// src/App.js

import { StatusBar } from 'expo-status-bar';
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';
import CounterNumber from './components/CounterNumber';
import IncreaseButton from './components/IncreaseButton';


export default function App() {
  return (
    
      <View style={styles.container}>
        <CounterNumber />
        <IncreaseButton />
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