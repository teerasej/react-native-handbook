
# อัพเดตส่วนของ User Interface ด้วย State และ React Hook

สามารถใช้คำสั่ง clone โปรเจคมาทำตามได้จากที่นี่ ถ้าต้องการ

```bash
git clone -b starter https://github.com/teerasej/react-native-simple-with-react-hook.git
```

จากนั้นเปิดไฟล์ App.js เพื่อเริ่มการทำงาน

## 1. import react hook `useState`

react hook เป็น module หนึ่งที่ติดมากับ react 16.8 เป็นต้นมา เราสามารถนำ react hook ที่ต้องการมาใช้ได้ผ่านคำสั่ง import 

```js
import { useState } from 'react';
```

## 2. ใช้ useState สร้างตัวแปร State

useState สามารถใช้สร้าง **ตัวแปร** และ **function** ที่ใช้อัพเดตตัวแปรนั้น การกำหนดค่าให้ตัวแปรผ่าน function ดังกล่าว เทียบเท่าการใช้คำสั่ง `setState()`

รูปแบบการใช้งาน react hook **useState** จะเป็นดังนี้ 

```
const [ชื่อตัวแปร, function ที่ใช้อัพเดตตัวแปร] = useState(ค่าเริ่มต้นของตัวแปร);
```

ในที่นี้เราจะประกาศ ตัวแปร state ขึ้นมา 2 ตัว นั่นคือ `isFirstTime` และ `isSignedIn`

```js
export default function App() {

  const [isFirstTime, setFirstTime] = useState(true);
  const [isSignedIn, setSignIn] = useState(false);
```

## 3. เรียกใช้งาน function เพื่ออัพเดตค่าในตัวแปร state

ดังนั้นหากต้องการ อัพเดตค่าให้ตัวแปร state เราจะทำผ่าน function ที่สร้างขึ้นมาโดยเฉพาะ

เช่นเราจะมีการอัพเดตค่าใน `setSignIn()` function และ `setFirstTime()` ที่ถูกสร้างขึ้นมา

```js
let signIn = () => {
    setFirstTime(false);
    setSignIn(true);
}

let signOut = () => {
    setSignIn(false);
}
```

## 4. ตัวแปร State ที่เปลี่ยนไป จะทำให้ function component ทำงานไม่เหมือนเดิม

การใช้ function อัพเดตค่าตัวแปร state จะทำให้ function component render ตัวเองใหม่ ซึ่งการเอาตัวแปร state มาใช้กำหนดเงื่อนไขการทำงานของ component จะทำให้เราควบคุมการแสดงผลของ component ได้

เช่น ในทีนี้ เราใช้ตัวแปร `isFirstTime` และ `isSignedIn` ในการกำหนดหน้าตาให้ได้ตามเงื่อนไข

```jsx
if (isFirstTime) {
      buttonComponent = (
        <Button
          title="ลงชื่อเข้าใช้"
          onPress={signIn}
        />
      )
    } else {
      if (isSignedIn) {
        buttonComponent = (
          <Button
            title="ออกจากระบบ"
            onPress={signOut}
          />
        )
        wordComponent = (
          <Hello name="พล" website="nextflow.in.th" />
        )
      } else {
        buttonComponent = (
          <Button
            title="ลงชื่อเข้าใช้"
            onPress={signIn}
          />
        )
        wordComponent = (
          <Goodbye name="พล" website="nextflow.in.th" />
        )
      }
    }
```

# ไฟล์เต็ม App.js 

```jsx
import { StatusBar } from 'expo-status-bar';
import React, {useState} from 'react';
import { StyleSheet, Text, View, Button } from 'react-native';
import Goodbye from './Goodbye';
import Hello from './Hello';

export default function App() {

  const [isFirstTime, setFirstTime] = useState(true);
  const [isSignedIn, setSignIn] = useState(false);
  
  let popUpAlert = () => {
    Alert.alert('oops...', 'you do something');
  }

  let signIn = () => {
    setFirstTime(false);
    setSignIn(true);
  }

  let signOut = () => {
    setSignIn(false);
  }

 
    let username = 'พล';
    let website = 'nextflow.in.th'

    let buttonComponent;
    let wordComponent;


    if (isFirstTime) {
      buttonComponent = (
        <Button
          title="ลงชื่อเข้าใช้"
          onPress={signIn}
        />
      )
    } else {
      if (isSignedIn) {
        buttonComponent = (
          <Button
            title="ออกจากระบบ"
            onPress={signOut}
          />
        )
        wordComponent = (
          <Hello name="พล" website="nextflow.in.th" />
        )
      } else {
        buttonComponent = (
          <Button
            title="ลงชื่อเข้าใช้"
            onPress={signIn}
          />
        )
        wordComponent = (
          <Goodbye name="พล" website="nextflow.in.th" />
        )
      }
    }

    return (
      <View style={styles.container}>
        {wordComponent}
        {buttonComponent}
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

# ไฟล์เต็ม Goodbye.js 

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

# ไฟล์เต็ม Hello.js 

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