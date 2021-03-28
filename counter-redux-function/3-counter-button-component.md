
# สร้างส่วนปุ่มเพิ่มจำนวน Counter

## 1. สร้างไฟล์ `IncreaseButton.js`

```jsx
// rnfs
// IncreaseButton.js

import React from 'react'
import { Button, StyleSheet, Text, View } from 'react-native'

export default function IncreaseButton() {

    const increase = () => {
        console.log('increase...')
    }

    return (
        <View>
            <Button title="เพิ่ม" onPress={increase}></Button>
        </View>
    )
}

const styles = StyleSheet.create({})


```

## 2. นำมาแสดงในแอพ

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
      <CounterNumber/>

      {/* แสดงปุ่มเพิ่มจำนวน */}
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