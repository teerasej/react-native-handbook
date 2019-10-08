
# Event ใน Function component 

- การใช้งาน User Interface จะทำให้เกิด event และเราสามารถนำ function มารับ event พวกนี้ได้

## 1. สร้าง Function สำหรับใช้กับ Event 

เปิดไฟล์ `App.js` และเขียน function ชื่อ `popUpAlert`

```js
import { StyleSheet, Text, View, Button, Alert } from 'react-native';

const popUpAlert = () => {
  Alert.alert('oops...','you do something');
}
```

## 2. กำหนด function ให้กับ event ของปุ่มชื่อ onPress

```jsx
<Button title="ลงชื่อเข้าใช้" color={styles.signInButton.color}
        onPress={popUpAlert}
      />
```

## A. ไฟล์เต็ม App.js

```jsx
import React from 'react';
import { StyleSheet, Text, View, Button, Alert } from 'react-native';
import Hello from "./Hello";
import Goodbye from "./Goodbye";

const popUpAlert = () => {
  Alert.alert('oops...','you do something');
}

export default function App() {

  let username = 'พล';
  let website = 'nextflow.in.th'
  
  return (
    <View style={styles.container}>
      <Text>Open up App.js to start working on your app!</Text>
      <Hello name="พล" website="nextflow.in.th"/>
      <Goodbye name={username} website={website}/>
      <Button title="ลงชื่อเข้าใช้" color={styles.signInButton.color}
        onPress={popUpAlert}
      />
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
  helloText: {
    fontSize: 30,
    color: 'green'
  },
  signInButton: {
    color: 'orange'
  }
});

```