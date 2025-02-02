

# Function Component

- เราสามารถกำหนด Function ที่ return ค่า เป็น JSX สำหรับการสร้าง Tag Component ของเราได้
- Function component จะดูเหมือน function ของ javascript ทั่วไปมาก แต่มันเป็น function ที่ return ค่าออกมาเป็น JSX โดยตรง

## 0. Reset ไฟล์ index.tsx

เปิดไฟล์ `app/index.tsx`

> **ให้แทนที่ทั้งไฟล์นี้ด้วยโค้ดด้านล่าง**

```jsx
// app/index.tsx

import React from "react";
import { Text, View } from "react-native";

export default function Index() {
  return (
    <View
      style={{
        flex: 1,
        justifyContent: "center",
        alignItems: "center",
      }}
    >
      <Text>Edit app/index.tsx to edit this screen.</Text>
    </View>
  );
}
```

## 1. สร้าง Component ใหม่ ด้วยการเขียนแบบ Function

เริ่มจากในไฟล์​ `app/index.tsx` ให้ลองสร้าง Function component แบบด้านล่าง

```jsx
// app/index.tsx

import React from "react";
import { Text, View } from "react-native";

// สร้าง 'Hello' component ในรูปแบบ Function component
let Hello = () => {
  return (
    <Text>Hello</Text>
  )
}


export default function Index() {
  return (
    <View
      style={{
        flex: 1,
        justifyContent: "center",
        alignItems: "center",
      }}
    >
      {/* ใช้งาน Hello component */}
      <Hello/>
    </View>
  );
}
```

## 2. เขียน Component แยกไฟล์

เริ่มจากสร้างไฟล์ `app/Hello.tsx` และเขียนโค้ดด้านล่าง

ใช้ snippet ชื่อ **rnfs** ได้ 

```jsx
// app/Hello.tsx

import { StyleSheet, Text, View } from 'react-native'
import React from 'react'

export default function Hello() {
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

```

และกลับมาที่ไฟล์ `app/index.tsx` แล้วเขียนคำสั่ง Import component มาจากไฟล์ `Hello.tsx`

```js
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
      {/* ใช้งาน Hello component */}
      <Hello/>
    </View>
  );
}
```

จะเห็นว่าเราได้หน้าตาแอพเหมือนเดิม แต่ตอนนี้เราแยก component ไปไว้อีกไฟล์ต่างหากแล้ว เราจึงสามารถเอาไปใช้ในไฟล์อื่นนอกเหนือจาก app/index.tsx ได้อีก (ทางเทคนิคเราเรียกไฟล์พวกนี้ว่า **module**) 

## A. ไฟล์เต็ม app/index.tsx

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
      {/* ใช้งาน Hello component */}
      <Hello/>
    </View>
  );
}
```

## B. ไฟล์เต็ม Hello.tsx

```jsx
// app/Hello.tsx

import { StyleSheet, Text, View } from 'react-native'
import React from 'react'

export default function Hello() {
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

```