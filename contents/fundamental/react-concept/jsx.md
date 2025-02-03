
# 4. Using JSX

## 1. Use JSX in React Native

- JSX ใช้กำหนดส่วน User Interface ของ Component นั้นๆ 
- ลักษณะของ JSX จะคล้ายกับ HTML มาก 
- JSX จะเป็นส่วนของ User Interface ที่เขียนลงไปใน JavaScript โดยตรง 

โดยทุก Component จะมีการ return JSX ไม่ว่าจะเป็นการเขียนแบบไหนก็ตาม

### ทดลองกำหนด User Interface เพิ่มเติม

เปิดไฟล์ `app/index.tsx`

จะเห็นว่ามี Component ตัวหนึ่งชื่อ **Home** อยู่ 

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

เราจะลองเพิ่ม JSX ที่เขียนด้วย **Text** component ตามด้านล่าง

```jsx
<Text>Hello</Text>
```

จะกลายเป็นแบบนี้ 

```jsx
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

### 2.1 ใช้งาน StyleSheet object ใน React Native

1. เราจะเรียกใช้ StyleSheet จาก react-native module และย้ายค่า CSS ออกมาจาก View component มาไว้ที่ StyleSheet object ชื่อ `styles` แทน 
2. นำค่า `styles.container` มาใช้งานใน `style` attribute ของ View component

```js
// app/index.tsx
import React from "react";

// import StyleSheet มาจาก react-native
import { Text, View, StyleSheet } from "react-native";

export default function Index() {
  return (
    // นำค่า styles.container มาใช้งาน
    <View
      style={styles.container}
    >
      <Text>Hello</Text>
    </View>
  );
}

// สร้าง StyleSheet object ชื่อ styles และกำหนดค่าให้กับ property ชื่อ container 
const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
  }
});
```

### 2.2 ทดลองเพิ่ม Style ให้กับ Text Component

1. เพิ่มค่า `styles.text` ลงไปใน `StyleSheet.create({})` ดังด้านล่าง
2. นำค่า `styles.text` มาใช้งานใน `style` attribute ของ Text component

```jsx
// app/index.tsx

import React from "react";
import { Text, View, StyleSheet } from "react-native";

export default function Index() {
  return (
    <View style={styles.container}>
      <Text style={styles.text}>Hello</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
  },
  text: {
    color: 'purple',
    fontSize: 24,
  }
});
```

ทดสอบดูผลการทำงาน

## 3. FlexBox Layout

ทำการปรับ User Interface และใช้ Flex stylesheet ตามภาพด้านล่าง

```jsx
// app/index.tsx

import React from "react";
import { Text, View, StyleSheet } from "react-native";

export default function Index() {
  return (
    <View style={styles.container}>
      <View style={styles.header}>
        <Text style={styles.headerText}>Home</Text>
      </View>
      <View style={styles.content}>
        <Text style={styles.contentText}>Content</Text>
      </View>
      <View style={styles.footer}>
        <Text style={styles.footerText}>Footer</Text>
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    flexDirection: 'column',
  },
  header: {
    flex: 1,
    backgroundColor: 'purple',
    justifyContent: 'center',
    alignItems: 'center',
  },
  headerText: {
    fontSize: 24,
    color: 'white',
  },
  content: {
    flex: 8,
    backgroundColor: 'white',
    justifyContent: 'center',
    alignItems: 'center',
  },
  contentText: {
    fontSize: 24,
    color: 'black',
  },
  footer: {
    flex: 1,
    backgroundColor: 'green',
    justifyContent: 'center',
    alignItems: 'center',
  },
  footerText: {
    fontSize: 24,
    color: 'white',
  },
});
```

1. ทดสอบปรับ Stylesheet **content** จาก `flex:2` เป็น 8 และ 12
2. ปรับ Stylesheet content และ contentText ตามด้านล่าง

  ```jsx
  content: {
    flex: 12,
    backgroundColor: 'white',
    justifyContent: 'flex-start',
    alignItems: 'flex-start',
  },
  contentText: {
    fontSize: 24,
    color: 'black',
  },
  ```


## 4. การนำ JSX Component ของ module อื่นๆ มาใช้งาน

- JSX Component พื้นฐาน จะมีให้จาก react-native module 
- การจะเรียกใช้ component ต้องมีการเขียนคำสั่ง import พร้อมระบุชื่อให้ถูกต้อง 
- ดูรายละเอียดเพิ่มเติมได้จาก [React Native API](https://facebook.github.io/react-native/docs/activityindicator)

### 4.1 SafeArea

ทำการเรียกใช้งาน SafeArea จาก `react-native` module

```jsx
// app/index.tsx

import React from "react";
import { View, Text, StyleSheet } from 'react-native';

export default function Index() {
  return (
    <View style={styles.container}>

      {/* SafeArea ครอบ View ทั้งหมด */}
      <SafeAreaView style={styles.safeArea}>
          <View style={styles.header}>
            <Text style={styles.headerText}>Header</Text>
          </View>
          <View style={styles.content}>
            <Text style={styles.contentText}>Content</Text>
          </View>
          <View style={styles.footer}>
            <Text style={styles.footerText}>Footer</Text>
          </View>
        </SafeAreaView>
    </View>
  );
}

const styles = StyleSheet.create({
  // เพิ่ม Stylesheet สำหรับ SafeArea ด้านล่าง กำหนด flex เป็น 1 เพื่อให้กินพื้นที่ด้านล่างทั้งหมด
  safeArea: {
    flex: 1,
    backgroundColor: 'green',
  },
  container: {
    flex: 1,
    flexDirection: 'column',
  },
  header: {
    flex: 1,
    backgroundColor: 'purple',
    justifyContent: 'center',
    alignItems: 'center',
  },
  headerText: {
    fontSize: 24,
    color: 'white',
  },
  content: {
    flex: 8,
    backgroundColor: 'white',
    justifyContent: 'center',
    alignItems: 'center',
  },
  contentText: {
    fontSize: 24,
    color: 'black',
  },
  footer: {
    flex: 1,
    backgroundColor: 'green',
    justifyContent: 'center',
    alignItems: 'center',
  },
  footerText: {
    fontSize: 24,
    color: 'white',
  },
});
```

ทดสอบการทำงาน **เราน่าจะเห็น Error แดงเถือก ประมาณว่า "หา SafeAreaView" ไม่เจอ **

ขึ้นมาด้านบนในส่วนคำสั่ง import และเขียนคำสั่ง import ที่ 3 ลงไป

```js
// app/index.tsx

import React from "react";
// เรียกใช้ SafeAreaView จาก react-native
import { SafeAreaView, View, Text, StyleSheet } from 'react-native';

export default function Index() {
  return (
    <View style={styles.container}>
      <SafeAreaView style={styles.safeArea}>
          <View style={styles.header}>
            <Text style={styles.headerText}>Header</Text>
          </View>
          <View style={styles.content}>
            <Text style={styles.contentText}>Content</Text>
          </View>
          <View style={styles.footer}>
            <Text style={styles.footerText}>Footer</Text>
          </View>
        </SafeAreaView>
    </View>
  );
}

const styles = StyleSheet.create({
  safeArea: {
    flex: 1,
    backgroundColor: 'green',
  },
  container: {
    flex: 1,
    flexDirection: 'column',
  },
  header: {
    flex: 1,
    backgroundColor: 'purple',
    justifyContent: 'center',
    alignItems: 'center',
  },
  headerText: {
    fontSize: 24,
    color: 'white',
  },
  content: {
    flex: 8,
    backgroundColor: 'white',
    justifyContent: 'center',
    alignItems: 'center',
  },
  contentText: {
    fontSize: 24,
    color: 'black',
  },
  footer: {
    flex: 1,
    backgroundColor: 'green',
    justifyContent: 'center',
    alignItems: 'center',
  },
  footerText: {
    fontSize: 24,
    color: 'white',
  },
});
```

### 4.2 Button

เพิ่ม [`Button` Component](https://gluestack.io/ui/docs/components/button) ลงใน JSX ของ Index Component 

```jsx
// app/index.tsx

import React from "react";

// เรียกใช้ Button และ ButtonText จาก '@/components/ui/button'
import { Button, ButtonText } from '@/components/ui/button';
import { SafeAreaView, View, Text, StyleSheet } from 'react-native';

export default function Index() {
  return (
    <View style={styles.container}>
      <SafeAreaView style={styles.safeArea}>
        <View style={styles.header}>
          <Text style={styles.headerText}>Header</Text>
        </View>
        <View style={styles.content}>
          <Text style={styles.contentText}>Content</Text>

          {/* เพิ่ม Button Component ลงไป */}
          <Button size="md" variant="solid" action="primary">
            <ButtonText>Hello World!</ButtonText>
          </Button>
        </View>
        <View style={styles.footer}>
          <Text style={styles.footerText}>Footer</Text>
        </View>
      </SafeAreaView>
    </View>
  );
}

const styles = StyleSheet.create({
  safeArea: {
    flex: 1,
    backgroundColor: 'green',
  },
  container: {
    flex: 1,
    flexDirection: 'column',
  },
  header: {
    flex: 1,
    backgroundColor: 'purple',
    justifyContent: 'center',
    alignItems: 'center',
  },
  headerText: {
    fontSize: 24,
    color: 'white',
  },
  content: {
    flex: 8,
    backgroundColor: 'white',
    justifyContent: 'center',
    alignItems: 'center',
  },
  contentText: {
    fontSize: 24,
    color: 'black',
  },
  footer: {
    flex: 1,
    backgroundColor: 'green',
    justifyContent: 'center',
    alignItems: 'center',
  },
  footerText: {
    fontSize: 24,
    color: 'white',
  },
});
```



### Tips

เราสามารถเขียนรวม import หลายๆ Component จาก module เดียวกันได้แบบนี้ 

```js
import React from 'react';
import { StyleSheet, Text, View, Button } from 'react-native';
```




## A. ไฟล์สมบูรณ์ App.js 

```jsx
 // app/index.tsx

import { Button, ButtonText } from '@/components/ui/button';
import { SafeAreaView, View, Text, StyleSheet } from 'react-native';

export default function Index() {
  return (
    <View style={styles.container}>
      <SafeAreaView style={styles.safeArea}>
        <View style={styles.header}>
          <Text style={styles.headerText}>Header</Text>
        </View>
        <View style={styles.content}>
          <Text style={styles.contentText}>Content</Text>
          <Button size="md" variant="solid" action="primary">
            <ButtonText>Hello World!</ButtonText>
          </Button>
        </View>
        <View style={styles.footer}>
          <Text style={styles.footerText}>Footer</Text>
        </View>
      </SafeAreaView>
    </View>
  );
}

const styles = StyleSheet.create({
  safeArea: {
    flex: 1,
    backgroundColor: 'green',
  },
  container: {
    flex: 1,
    flexDirection: 'column',
  },
  header: {
    flex: 1,
    backgroundColor: 'purple',
    justifyContent: 'center',
    alignItems: 'center',
  },
  headerText: {
    fontSize: 24,
    color: 'white',
  },
  content: {
    flex: 8,
    backgroundColor: 'white',
    justifyContent: 'center',
    alignItems: 'center',
  },
  contentText: {
    fontSize: 24,
    color: 'black',
  },
  footer: {
    flex: 1,
    backgroundColor: 'green',
    justifyContent: 'center',
    alignItems: 'center',
  },
  footerText: {
    fontSize: 24,
    color: 'white',
  },
});


```
