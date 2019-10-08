
# 4. Using JSX

## 1. Use JSX in React Native

- JSX ใช้กำหนดส่วน User Interface ของ Component นั้นๆ 
- ลักษณะของ JSX จะคล้ายกับ HTML มาก 
- JSX จะเป็นส่วนของ User Interface ที่เขียนลงไปใน JavaScript โดยตรง 

โดยทุก Component จะมีการ return JSX ไม่ว่าจะเป็นการเขียนแบบไหนก็ตาม

### ทดลองกำหนด User Interface เพิ่มเติม

เปิดไฟล์ `App.js`

จะเห็นว่ามี Component ตัวหนึ่งชื่อ **App** อยู่

```jsx
export default function App() {
  return (
    <View style={styles.container}>
      <Text>Open up App.js to start working on your app!</Text>
    </View>
  );
}
```

เราจะลองเพิ่ม JSX ที่เขียนด้วย **Text** component ตามด้านล่าง

```jsx
<Text>Hello</Text>
```

จะกลายเป็นแบบนี้ 

```jsx
export default function App() {
  return (
    <View style={styles.container}>
      <Text>Open up App.js to start working on your app!</Text>
      <Text>Hello</Text>
    </View>
  );
}
```

ทดสอบดูผลการทำงาน

## 2. กำหนด Style ให้กับ JSX Component 

- ถ้าใน HTML มี CSS ใน JSX ก็มี Stylesheet object 
- การเขียน StyleSheet object จะเป็นเหมือนการเขียน JavaScript object นั่นคือคล้ายกับการกำหนดค่า CSS 
- การนำมาใช้ จะเป็นการกำหนดค่าลงในค่า `style` attribute ของ JSX Component โดยตรง

### ทดสอบ

เพิ่มค่า **helloText** ลงไปใน `StyleSheet.create({})` ดังด้านล่าง

```js
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
  }
});
```

ขึ้นมาที่ JSX ของ App Component และกำหนดค่า `styles.helloText` ลงไปใน `style` attribute ของ Component

```jsx
export default function App() {
  return (
    <View style={styles.container}>
      <Text>Open up App.js to start working on your app!</Text>
      <Text style={styles.helloText}>Hello</Text>
    </View>
  );
}
```

ทดสอบดูผลการทำงาน

## 3. การนำ JSX Component ของ module อื่นๆ มาใช้งาน

- JSX Component พื้นฐาน จะมีให้จาก react-native module 
- การจะเรียกใช้ component ต้องมีการเขียนคำสั่ง import พร้อมระบุชื่อให้ถูกต้อง 
- ดูรายละเอียดเพิ่มเติมได้จาก [React Native API](https://facebook.github.io/react-native/docs/activityindicator)


### ทดสอบ

เพิ่ม `Button` Component ลงใน JSX ของ App Component 

```jsx
export default function App() {
  return (
    <View style={styles.container}>
      <Text>Open up App.js to start working on your app!</Text>
      <Text style={styles.helloText}>Hello</Text>
      <Button title="ลงชื่อเข้าใช้"/>
    </View>
  );
}
```

ทดสอบการทำงาน เราน่าจะเห็น Error แดงเถือก ประมาณว่า "หา Button" ไม่เจอ 

ขึ้นมาด้านบนในส่วนคำสั่ง import และเขียนคำสั่ง import ที่ 3 ลงไป

```js
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';
import { Button } from 'react-native';
```

บันทึกไฟล์ และทดสอบการทำงาน จะเห็นว่าเรามีปุ่มแล้ว 

### Tips

เราสามารถเขียนรวม import หลายๆ Component จาก module เดียวกันได้แบบนี้ 

```js
import React from 'react';
import { StyleSheet, Text, View, Button } from 'react-native';
```

## 4. ใช้ StyleSheet กับปุ่ม Button component 

เพิ่มค่า **signInButton** ลงไปใน `StyleSheet.create({})` ดังด้านล่าง

```js
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

จากนั้นเอาค่า `styles.signInButton.color` มาใช้กับ `<Button ... color={}/>`

```jsx
<Button title="ลงชื่อเข้าใช้" color={styles.signInButton.color}/>
```

จากนั้นทดสอบการใช้งาน

ขึ้นมาที่ JSX ของ App Component และกำหนดค่า `styles.helloText` ลงไปใน `style` attribute ของ Component

```jsx
export default function App() {
  return (
    <View style={styles.container}>
      <Text>Open up App.js to start working on your app!</Text>
      <Text style={styles.helloText}>Hello</Text>
    </View>
  );
}
```

## A. ไฟล์สมบูรณ์ App.js 

```jsx
import React from 'react';
import { StyleSheet, Text, View, Button } from 'react-native';

export default function App() {
  return (
    <View style={styles.container}>
      <Text>Open up App.js to start working on your app!</Text>
      <Text style={styles.helloText}>Hello</Text>
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
    color: 'black'
  }
});

```