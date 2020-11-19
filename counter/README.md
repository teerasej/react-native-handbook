
# Counter Application

![2019-10-23_10-11-17](https://user-images.githubusercontent.com/85179/67353820-bb15dc80-f57d-11e9-805e-b73b00a24397.gif)


## 1. Download Project

- [โปรเจค Starter](https://www.dropbox.com/s/nsuhrm9scjkwi4y/counter-starter.zip?dl=0)
- [โปรเจค Finish](https://www.dropbox.com/s/4i9np244ltgk2xc/counter-finish.zip?dl=0)

- [โปรเจค Starter แบบ Function component](https://www.dropbox.com/s/u9z7ozz5i9b0oak/counter-function-component-start.zip?dl=0)
- [โปรเจค Finish แบบ Function component](https://www.dropbox.com/s/8ewhtp3zqtce6r2/counter-function-component-finish.zip?dl=0)

## 2. เปิดโฟลเดอร์โปรเจคแล้วรันคำสั่งติดตั้ง node modules

```bash
npm install 
```

## 3. กำหนด StyleSheet property สำหรับ Text component

```js
const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
//   ตั้งชื่อว่า number
  number: {
    fontSize: 30
  }
});
```

จากนั้นเอามาใช้ใน JSX ของ App Component

```jsx
export default class App extends React.Component {

  state = {

  }

  render() {
        return (
            <View style={styles.container}>
                <Text style={styles.number}>0</Text>
                {/* วางปุ่มตรงนี้ */}
            </View>
        );
  }
  
}
```

## 4. ทำการ import Button component จาก react-native module มาใช้งาน

```jsx
import { StyleSheet, Text, View, Button } from 'react-native';

export default class App extends React.Component {

  state = {

  }

  render() {
        return (
            <View style={styles.container}>
                <Text style={styles.number}>0</Text>
                {/* วางปุ่มตรงนี้ */}
                <Button title="เพิ่ม">
            </View>
        );
  }
  
}
```



## 5. สร้าง function และใช้กับ event `onPress` props ของ Button

```jsx
export default class App extends React.Component {

  state = {

  }

  // function ที่ต้องการใช้งาน
  increaseNumber = () => {
    console.log('เพิ่มจำนวน');
  }

  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.number}>0</Text>

        {/* กำหนด function ให้ event onPress */}
        <Button title="เพิ่ม" onPress={this.increaseNumber}/>
      </View>
    );
  }
```

## 6. กำหนดค่าใน State เริ่มต้น และเอามาใช้ใน JSX

```jsx
export default class App extends React.Component {
  
  // กำหนดค่า counter เป็น 0 เพราะจะเอามาแสดงใน JSX เป็นจำนวนนับ
  state = {
    counter: 0
  }
```

เสร็จแล้ว นำค่า `this.state.counter` มาใช้กับ JSX 

```jsx
render() {

    // ดึงค่ามาเก็บไว้ในตัวแปร จะได้เรียกใช้ง่าย อ่านง่ายใน JSX
    let currentCounter = this.state.counter;

    return (
      <View style={styles.container}>

        {/* แสดงค่า current counter ตรงนี้ */}
        <Text style={styles.number}>{currentCounter}</Text>
        
        <Button title="เพิ่ม" onPress={this.increaseNumber}/>
      </View>
    );
  }
```

## 7. กำหนดค่าให้ state ใหม่ เพื่อให้เกิดการ render App Component อีกครั้ง

```jsx
increaseNumber = () => {
    console.log('เพิ่มจำนวน');
    let currentCounter = this.state.counter;
    this.setState({
      counter: currentCounter + 1
    })
} 
```

## A. ไฟล์เต็ม App.js 

```jsx
import React from 'react';
import { StyleSheet, Text, View, Button } from 'react-native';

export default class App extends React.Component {

  state = {
    counter: 0
  }

  increaseNumber = () => {
    console.log('เพิ่มจำนวน');
    let currentCounter = this.state.counter;
    this.setState({
      counter: currentCounter + 1
    })
  }

  render() {

    let currentCounter = this.state.counter;

    return (
      <View style={styles.container}>
        <Text style={styles.number}>{currentCounter}</Text>
        <Button title="เพิ่ม" onPress={this.increaseNumber}/>
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
  number: {
    fontSize: 30
  }
});

```
