
# Props

- การส่งค่าเข้ามาใน `props` ของ component จะดูเหมือน HTML attribute ตามปกติ
- ค่า HTML attribute จะเป็นชื่อของ property ใน `props` เช่น `<Nextflow logo="brand.jpg">` ก็จะเป็น `props.logo` 

## การส่งค่า props

เราสามารถกำหนดชื่อ และค่าของ attribute ลงไปใน JSX ได้เลย

```jsx
<Hello name="พล"/>
```

และสามารถแทรกค่าตัวแปรลงไปใน JSX ได้ผ่านเครื่องหมาย expression หรือ `{}`

```jsx
let username = 'nextflow';

<Goodbye name={username}/>
```

> สังเกตว่า ถ้าเราแทรกค่าตัวแปรเข้าไปใน attribute เราไม่จำเป็นต้องใส่เครื่องหมาย `""`

## การเข้าถึงค่า Props ใน Function component 

- ค่าที่ถูกส่งเข้ามาผ่าน Attribute จะสามารถเข้าถึงได้ ในรูปแบบของ object ตัวหนึ่งที่ชื่อว่า `props` 
- ชื่อของ attribute จะกลายเป็นค่า property ของ object เช่นถ้าส่งเป็น `<Hello name="พล">` ก็จะเป็น `props.name` จากในใน component

เปิดไฟล์ **Hello.js** และปรับโค้ดตามด้านล่าง

```jsx
const Hello = (props) => {
    return (
        <View>
            <Text style={styles.helloText}>Hello {props.name}</Text>
        </View>
    )
}
```

ทดสอบใช้งาน

## การเข้าถึงค่า Props ใน Class component 

- ค่าที่ถูกส่งเข้ามาผ่าน Attribute จะสามารถเข้าถึงได้ ในรูปแบบของ property ใน Class  ตัวหนึ่งที่ชื่อว่า `this.props` 
- ชื่อของ attribute จะกลายเป็นค่า property ของ object เช่นถ้าส่งเป็น `<Goodbye name="พล">` ก็จะเป็น `this.props.name` จากในใน component

เปิดไฟล์ **Goodbye.js** และปรับโค้ดตามด้านล่าง

```jsx
export class Goodbye extends Component {
    render() {
        return (
            <View>
                <Text style={styles.defaultText}>Goodbye {this.props.name}</Text>
            </View>
        )
    }
}
```

ทดสอบใช้งาน

## De-construct object props

- เราสามารถประกาศชื่อให้กับค่าที่ส่งเข้ามาโดยตรง และไม่ต้องเรียกผ่าน `props` หรือ `this.props` ได้ด้วย 
- ผลจากการทำวิธีนี้ เราสามารถควบคุมค่าที่ส่งเข้ามาใน Component ได้โดยตรง ไม่ dynamic เมื่อการเรียกใช้ค่าผ่าน `props`
- เช่น ถ้าเราส่งค่าเข้ามา `<Hello name="พล" website="nextflow">` ถ้าเราต้องการสร้างตัวแปรจากค่าที่ส่งเข้ามาทาง props สามารถเขียนได้แบบนี้

### แบบ Function component 

```jsx
// <Hello name="พล" website="nextflow">

// Deconstruct object props โดยตรง
const Hello = ({ name, website}) => {
    return (
        <View>
            {/* นำชื่อตัวแปรที่ได้จากการ deconstruct มาใช้ */}
            <Text style={styles.helloText}>Hello {name} from {website}</Text>
        </View>
    )
}
```

### แบบ Class component 

```jsx
export class Goodbye extends Component {
    render() {

        // Deconstruct this.props โดยตรง
        const { name, website } = this.props;

        return (
            <View>
                {/* นำชื่อตัวแปรที่ได้จากการ deconstruct มาใช้ */}
                <Text style={styles.defaultText}>Goodbye {name} from {website}</Text>
            </View>
        )
    }
}
```


## A. ไฟล์เต็ม App.js

```jsx
import React from 'react';
import { StyleSheet, Text, View, Button } from 'react-native';
import Hello from "./Hello";
import Goodbye from "./Goodbye";



export default function App() {

  let username = 'พล';
  let website = 'nextflow.in.th'
  
  return (
    <View style={styles.container}>
      <Text>Open up App.js to start working on your app!</Text>
      <Hello name="พล" website="nextflow.in.th"/>
      <Goodbye name={username} website={website}/>
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

const Hello = ({ name, website}) => {
    return (
        <View>
            <Text style={styles.helloText}>Hello {name} from {website}</Text>
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

## C. ไฟล์เต็ม Goodbye.js

```jsx

import React, { Component } from 'react'
import { Text, View, StyleSheet } from 'react-native'

export class Goodbye extends Component {
    render() {
        const { name, website } = this.props;

        return (
            <View>
                <Text style={styles.defaultText}>Goodbye {name} from {website}</Text>
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