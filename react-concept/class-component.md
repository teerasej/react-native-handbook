
# Class Component

- รูปแบบของ Class component คือการเขียน Component เชิง OOP (Object-Oriented Programming)
- ใน Class component อย่างน้อยจะมี method (function) หนึ่ง ชื่อ `render()` ถูกเขียนไว้
- `render()` จะ return ค่าเป็น JSX เสมอ

## 1. สร้าง Class Component 

สร้างไฟล์ `Goodbye.js`

```jsx
import React, { Component } from 'react'
import { Text, View, StyleSheet } from 'react-native'

export class Goodbye extends Component {
    render() {
        return (
            <View>
                <Text style={styles.defaultText}>Goodbye</Text>
            </View>
        )
    }
}

const styles = StyleSheet.create({
    defaultText: {
        fontSize: 30,
        color: 'red'
    }
});

export default Goodbye
```

## 2. ใช้ Goodbye component ใน App.js 

เปิดไฟล์ `App.js` และเพิ่มคำสั่ง import 

```js
import Goodbye from "./Goodbye";
```

จากนั้นเรียกใช้ใน App.js 

```jsx
export default function App() {
  return (
    <View style={styles.container}>
      <Text>Open up App.js to start working on your app!</Text>
      <Hello/>
      {/* เรียกใช้ Goodbye component */}
      <Goodbye/>
      <Button title="ลงชื่อเข้าใช้" color={styles.signInButton.color}/>
    </View>
  );
}
```

## A. ไฟล์เต็ม App.js 

```jsx
import React from 'react';
import { StyleSheet, Text, View, Button } from 'react-native';
import Hello from "./Hello";
import Goodbye from "./Goodbye";


export default function App() {
  return (
    <View style={styles.container}>
      <Text>Open up App.js to start working on your app!</Text>
      <Hello/>
      <Goodbye/>
      <Button title="ลงชื่อเข้าใช้" color={styles.signInButton.color}/>
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