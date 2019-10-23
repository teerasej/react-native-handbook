
# Counter Application

## 1. Download Project

- [โปรเจค Starter](https://www.dropbox.com/s/fkr0h7v3qzcim4r/counter-starter.zip?dl=0)
- [โปรเจค Finish](https://www.dropbox.com/s/rxf2o45vidmturb/counter-finish.zip?dl=0)

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
