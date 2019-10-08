
# อัพเดตส่วนของ User Interface ด้วย State

- State เป็น object ตัวหนึ่งที่อยู่ใน component 
- เราใช้ค่าจาก state ในการอ้างอิงโค้ดการทำงานของ component ได้ รวมทั้งใน `render()` function
- การเปลี่ยนแปลงค่าใน state ต้องทำผ่านการ function ชื่อ `setState()` เท่านั้น
- เมื่อมีการเรียกใช้ `setState()` ตัว component จะทำการเรียกใช้ function `render()` ใหม่

## 1. ใช้ตัวแปรเก็บค่า JSX แทน 

เราจะประกาศตัวแปร 2 ตัว

1. `buttonComponent` เก็บ JSX ส่วนแสดงปุ่ม
2. `wordComponent` เก็บ JSX ส่วนแสดงคำทักทาย

และนำมาใช้ในส่วน JSX 

```jsx
return (
      <View style={styles.container}>
        {wordComponent}
        {buttonComponent}
      </View>
    );
```

## 2. กำหนดค่าใน state object 

```js
export default class App extends Component {

  state = {
    isFirstTime: true,
    isSignedIn: false
  }
```

## 3. เรียกใช้ค่าจาก state ในส่วน `render()`

เรียกค่าจาก `this.state` และใช้มันในการพิจารณากำหนด JSX ให้ตัวแปร `buttonComponent` และ `wordComponent`

```jsx
render() {

    //...

    const { isFirstTime, isSignedIn, isSignedOut } = this.state;

    if (isFirstTime) {
      buttonComponent = (
        <Button
          title="ลงชื่อเข้าใช้"
          onPress={this.signIn}
        />
      )
    } else {
      if (isSignedIn) {
        buttonComponent = (
          <Button
            title="ออกจากระบบ"
            onPress={this.signOut}
          />
        )
        wordComponent = (
          <Hello name="พล" website="nextflow.in.th" />
        )
      } else {
        buttonComponent = (
          <Button
            title="ลงชื่อเข้าใช้"
            onPress={this.signIn}
          />
        )
        wordComponent = (
          <Goodbye name="พล" website="nextflow.in.th" />
        )
      }
    }

    //...
}
```

## 4. สร้าง function ที่จะกำหนดค่าใหม่ให้ state 

ปุ่ม **ลงชื่อเข้าใช้** และ **ออกจากระบบ** มีการกำหนด function ให้ event onPress

ให้เรามาเขียน function ของทั้ง 2 ปุ่ม โดยมีการกำหนดค่าใหม่ ให้กับ state object

```js
signIn = () => {
    this.setState({
      isFirstTime: false,
      isSignedIn: true
    })
  }

  signOut = () => {
    this.setState({
      isSignedIn: false
    })
  }
```

ทดสอบการทำงาน

## A. ไฟล์เต็ม App.js 

```jsx
import React, { Component } from 'react';
import { StyleSheet, Text, View, Button, Alert } from 'react-native';
import Hello from "./Hello";
import Goodbye from "./Goodbye";

export default class App extends Component {

  state = {
    isFirstTime: true,
    isSignedIn: false
  }

  popUpAlert = () => {
    Alert.alert('oops...', 'you do something');
  }

  signIn = () => {
    this.setState({
      isFirstTime: false,
      isSignedIn: true
    })
  }

  signOut = () => {
    this.setState({
      isSignedIn: false
    })
  }

  render() {
    let username = 'พล';
    let website = 'nextflow.in.th'

    let buttonComponent;
    let wordComponent;

    const { isFirstTime, isSignedIn, isSignedOut } = this.state;

    if (isFirstTime) {
      buttonComponent = (
        <Button
          title="ลงชื่อเข้าใช้"
          onPress={this.signIn}
        />
      )
    } else {
      if (isSignedIn) {
        buttonComponent = (
          <Button
            title="ออกจากระบบ"
            onPress={this.signOut}
          />
        )
        wordComponent = (
          <Hello name="พล" website="nextflow.in.th" />
        )
      } else {
        buttonComponent = (
          <Button
            title="ลงชื่อเข้าใช้"
            onPress={this.signIn}
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