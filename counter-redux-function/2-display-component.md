
# สร้างส่วนแสดงจำนวน Counter

## 1. สร้างไฟล์ `CounterNumber.js`

```jsx
// snippet: rnfc
// CounterNumber.js


import React from 'react'
import { StyleSheet, Text, View } from 'react-native'

export default function CounterNumber() {
    return (
        <View>
            <Text style={styles.counter}>0</Text>
        </View>
    )
}

const styles = StyleSheet.create({
    counter: {
        fontSize: 60
    }
})

```


## 2. นำมาแสดงในแอพ

```jsx
// App.js

import { StatusBar } from 'expo-status-bar';
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';
import CounterNumber from './CounterNumber';

export default function App() {
  return (
    <View style={styles.container}>
      
    {/* แสดงตัวนับ */}
      <CounterNumber/>

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