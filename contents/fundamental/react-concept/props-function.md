# การเข้าถึงค่า Props ใน Function component 

- ค่าที่ถูกส่งเข้ามาผ่าน Attribute จะสามารถเข้าถึงได้ ในรูปแบบของ object ตัวหนึ่งที่ชื่อว่า `props` 
- ชื่อของ attribute จะกลายเป็นค่า property ของ object เช่นถ้าส่งเป็น `<Hello name="พล">` ก็จะเป็น `props.name` จากในใน component

## 1. กำหนด Data Type ให้ Props ของ Hello component ด้วย Interface


1. เปิดไฟล์ **app/Hello.tsx** และปรับโค้ดตามด้านล่าง

```jsx
// app/Hello.tsx

import { StyleSheet, Text, View } from 'react-native'
import React from 'react'


// สร้าง interface สำหรับเป็น data type เพื่อกำหนดชื่อ props ที่สามารถส่งเข้ามาได้
interface HelloProps {
  name: string;
}

// กำหนด HelloProps เป็น data type ของ props ที่ส่งเข้ามา
export default function Hello(props: HelloProps) {


  return (
    <View>
        <Text style={styles.helloText}>Hello, {props.name}</Text>
    </View>
  )
}

const styles = StyleSheet.create({
    helloText: {
      fontSize: 30,
      color: 'green'
    }
  });
```

2. บันทึกไฟล์ ให้สังเกตว่า ไฟล์ `app/index.tsx` จะมี error ทำให้เราต้องไปกำหนดค่า props ด้วย

```jsx
// app/index.tsx

import React from "react";
import { Text, View } from "react-native";
import Hello from "./Hello";



export default function Index() {
  return (
    <View
      style={{
        flex: 1,
        justifyContent: "center",
        alignItems: "center",
      }}
    >
      {/* ส่งค่าเข้าไปใน props name */}
      <Hello name="Nextflow"/>
    </View>
  );
}
```

3. บันทึกไฟล์ และรันโปรเจค จะเห็นข้อความ `Hello, Nextflow` ที่แสดงออกมา 

## 2. การใช้ nullable ใน Interface

1. ถ้าเราต้องการให้ค่า props นั้นเป็น nullable สามารถใช้ `?` ได้

```jsx
// app/Hello.tsx

import { StyleSheet, Text, View } from 'react-native'
import React from 'react'


interface HelloProps {
    // ให้ name เป็น nullable นั่นหมายความว่า ตัวแปรนี้สามารถเป็น null ได้ 
    name?: string;
}

export default function Hello(props: HelloProps) {

    // ใช้ nullable operator เพื่อให้แน่ใจว่าตัวแปร name ไม่เป็น null แน่นอน ไม่ว่า Props.name จะเป็น null หรือไม่
    const name = props.name || 'Guest';

    return (
        <View>
            <Text style={styles.helloText}>Hello, {name}</Text>
        </View>
    )
}

const styles = StyleSheet.create({
    helloText: {
        fontSize: 30,
        color: 'green'
    }
});

```

2. บันทึกไฟล์
3. การเปลี่ยงแปลงนี้ จะทำให้เราสามารถส่งค่า `null` เข้าไปได้ โดยไม่ทำให้เกิด error
4. ในไฟล์ `app/index.tsx` เราทดลองส่งค่า `null` เข้าไปใน `Hello` component ด้วยการไม่กำหนดค่า `name`
   
```jsx
// app/index.tsx

import React from "react";
import { Text, View } from "react-native";
import Hello from "./Hello";


export default function Index() {
  return (
    <View
      style={{
        flex: 1,
        justifyContent: "center",
        alignItems: "center",
      }}
    >
        {/* ส่งค่าเข้าไปใน props name */}
      <Hello name="Nextflow"/>

        {/* การไม่กำหนดค่า จะเป็นการส่งค่า null เข้าไป ซึ่งจะทำให้แสดงเป็นข้อความ 'Hello, Guest'*/}
      <Hello/>
    </View>
  );
}
```

5. บันทึกไฟล์ และทดสอบการแสดงผล จะเห็นข้อความ `Hello, Guest` แสดงออกมา




## 3. De-construct object props

- เราสามารถประกาศชื่อให้กับค่าที่ส่งเข้ามาโดยตรง และไม่ต้องเรียกผ่าน `props` หรือ `this.props` ได้ด้วย 
- ผลจากการทำวิธีนี้ เราสามารถควบคุมค่าที่ส่งเข้ามาใน Component ได้โดยตรง ไม่ dynamic เมื่อการเรียกใช้ค่าผ่าน `props`
- เช่น ถ้าเราส่งค่าเข้ามา `<Hello name="พล" website="nextflow">` ถ้าเราต้องการสร้างตัวแปรจากค่าที่ส่งเข้ามาทาง props สามารถเขียนได้แบบนี้

### แบบ Function component 

```jsx
// app/Hello.tsx

import { StyleSheet, Text, View } from 'react-native'
import React from 'react'

interface HelloProps {
    name?: string;
}

// ใช้ de-construct ค่าที่ส่งเข้ามา โดยตรง สังเกตว่า ไม่ต้องใช้ props.name แล้ว 
// และเนื่องจาก name เป็น nullable จึงต้องกำหนดค่า default ด้วย
export default function Hello({ name = 'Guest' }: HelloProps) {
    return (
        <View>
            <Text style={styles.helloText}>Hello, {name}</Text>
        </View>
    )
}

const styles = StyleSheet.create({
    helloText: {
        fontSize: 30,
        color: 'green'
    }
});

```