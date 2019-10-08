

# Function Component

- เราสามารถกำหนด Function ที่ return ค่า เป็น JSX สำหรับการสร้าง Tag Component ของเราได้
- Function component จะดูเหมือน function ของ javascript ทั่วไปมาก แต่มันเป็น function ที่ return ค่าออกมาเป็น JSX โดยตรง

## 1. สร้าง Component ใหม่ ด้วยการเขียนแบบ Function

เริ่มจากในไฟล์​ App.js ให้ลองสร้าง Function component แบบด้านล่าง

```jsx
import React from 'react';
import { StyleSheet, Text, View, Button } from 'react-native';

// สร้าง 'Hello' component ในรูปแบบ Function component
let Hello = () => {
  return (
    <Text style={styles.helloText}>Hello</Text>
  )
}

export default function App() {
  return (
    <View style={styles.container}>
      <Text>Open up App.js to start working on your app!</Text>
      {/* ใช้งาน Hello component */}
      <Hello/>
      <Button title="ลงชื่อเข้าใช้" color={styles.signInButton.color}/>
    </View>
  );
}
```

## 2. เขียน Component แยกไฟล์

เริ่มจากสร้างไฟล์ `Hello.js` และเขียนโค้ดด้านล่าง

ใช้ snippet ชื่อ **rnf** ได้ 

```jsx
import React from 'react'
import { View, Text, StyleSheet } from 'react-native'

const Hello = () => {
    return (
        <View>
            <Text style={styles.helloText}>Hello</Text>
        </View>
    )
}

const styles = StyleSheet.create({
  helloText: {
    fontSize: 30,
    color: 'green'
  }
});

export default Hello
```

และกลับมาที่ไฟล์ `App.js` แล้วเขียนคำสั่ง Import component มาจากไฟล์ `Hello.js`

```js
import Hello from "./Hello";
```

จะเห็นว่าเราได้หน้าตาแอพเหมือนเดิม แต่ตอนนี้เราแยก component ไปไว้อีกไฟล์ต่างหากแล้ว เราจึงสามารถเอาไปใช้ในไฟล์อื่นนอกเหนือจาก App.js ได้อีก (ทางเทคนิคเราเรียกไฟล์พวกนี้ว่า **module**) 

## A. ไฟล์เต็ม App.js

```jsx
import React from 'react';
import { StyleSheet, Text, View, Button } from 'react-native';
import Hello from "./Hello";


export default function App() {
  return (
    <View style={styles.container}>
      <Text>Open up App.js to start working on your app!</Text>
      <Hello/>
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

## B. ไฟล์เต็ม Hello.js

```jsx
import React from 'react'
import { View, Text, StyleSheet } from 'react-native'

const Hello = () => {
    return (
        <View>
            <Text style={styles.helloText}>Hello</Text>
        </View>
    )
}

const styles = StyleSheet.create({
  helloText: {
    fontSize: 30,
    color: 'green'
  }
});

export default Hello
```