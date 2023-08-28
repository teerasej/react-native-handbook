
# แสดงทั้ง 2 component ไว้ใน App component


```jsx
// App.js

import { StatusBar } from 'expo-status-bar';
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';
import CounterNumber from './CounterNumber';
import IncreaseButton from './IncreaseButton';

export default function App() {
  return (
    <View style={styles.container}>
      
      
      {/* แสดงตัวนับ และปุ่มเพิ่มจำนวน */}
      <CounterNumber/>
      <IncreaseButton/>

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