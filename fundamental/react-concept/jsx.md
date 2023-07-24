
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

## 3. FlexBox Layout

ทำการปรับ User Interface และใช้ Flex stylesheet ตามภาพด้านล่าง

```jsx
// App.js

import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View } from 'react-native';

export default function App() {
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
    backgroundColor: 'blue',
    justifyContent: 'center',
    alignItems: 'center',
  },
  contentText: {
    fontSize: 24,
    color: 'white',
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
  helloText: {
    fontSize: 30,
    color: 'green'
  }
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


## 5. การนำ JSX Component ของ module อื่นๆ มาใช้งาน

- JSX Component พื้นฐาน จะมีให้จาก react-native module 
- การจะเรียกใช้ component ต้องมีการเขียนคำสั่ง import พร้อมระบุชื่อให้ถูกต้อง 
- ดูรายละเอียดเพิ่มเติมได้จาก [React Native API](https://facebook.github.io/react-native/docs/activityindicator)

### 5.1 SafeArea

ทำการเรียกใช้งาน SafeArea จาก `react-native` module

```jsx
  import React from 'react';
  import { SafeAreaView, View, Text, StyleSheet } from 'react-native';

export default function App() {
    return (
      <View style={styles.container}>
        {/* SafeArea ด้านบน */}
        <SafeAreaView style={styles.safeAreaTop}>
        </SafeAreaView>
        {/* SafeArea ด้านส่วนที่เหลือ  */}
        <SafeAreaView style={styles.safeAreaBottom}>
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
  };

  const styles = StyleSheet.create({
    // เพิ่ม Stylesheet สำหรับ SafeArea ด้านบน กำหนด flex เป็น 0 เพื่อไม่ให้กินพื้นที่ลงมาด้านล่าง
    safeAreaTop: {
      flex: 0,
      backgroundColor: 'red',
    },
    // เพิ่ม Stylesheet สำหรับ SafeArea ด้านล่าง กำหนด flex เป็น 1 เพื่อให้กินพื้นที่ด้านล่างทั้งหมด
    safeAreaBottom: {
      flex: 1,
      backgroundColor: 'green',
    },
    container: {
      flex: 1,
      flexDirection: 'column',
    },
    header: {
      flex: 1,
      backgroundColor: 'red',
      justifyContent: 'center',
      alignItems: 'center',
    },
    headerText: {
      fontSize: 24,
      color: 'white',
    },
    content: {
      flex: 18,
      backgroundColor: 'white',
      justifyContent: 'flex-start',
      alignItems: 'flex-start',
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
import React from 'react';

// เพิ่มส่วนนี้ เป็นการเรียกใช้ SafeAreaView component จาก module ชื่อ react-native
import { StyleSheet, Text, View,SafeAreaView  } from 'react-native';
```

### 5.2 Button

เพิ่ม `Button` Component ลงใน JSX ของ App Component 

```jsx
export default function App() {
  return (
    return (
      <View style={styles.container}>
        <SafeAreaView style={styles.safeAreaTop}>
        </SafeAreaView>
        <SafeAreaView style={styles.safeAreaBottom}>
          <View style={styles.header}>
            <Text style={styles.headerText}>Header</Text>
          </View>
          <View style={styles.content}>
            <Text style={styles.contentText}>Content</Text>

            {/* เพิ่มปุ่มในส่วน content */}
            <Button title="ลงชื่อเข้าใช้"/>
          </View>
          <View style={styles.footer}>
            <Text style={styles.footerText}>Footer</Text>
          </View>
        </SafeAreaView>
      </View>
    );
  );
}
```

ทดสอบการทำงาน **เราน่าจะเห็น Error แดงเถือก ประมาณว่า "หา Button" ไม่เจอ **

ขึ้นมาด้านบนในส่วนคำสั่ง import และเขียนคำสั่ง import ที่ 3 ลงไป

```js
import React from 'react';
import { StyleSheet, Text, View, SafeAreaView } from 'react-native';

// เพิ่มส่วนนี้ เป็นการเรียกใช้ Button component จาก module ชื่อ react-native
import { Button } from 'react-native';
```

บันทึกไฟล์ และทดสอบการทำงาน จะเห็นว่าเรามีปุ่มแล้ว 

### Tips

เราสามารถเขียนรวม import หลายๆ Component จาก module เดียวกันได้แบบนี้ 

```js
import React from 'react';
import { StyleSheet, Text, View, Button } from 'react-native';
```

## 6. ใช้ StyleSheet กับปุ่ม Button component 

เพิ่มค่า **signInButton** ลงไปใน `StyleSheet.create({})` ดังด้านล่าง

```js
const styles = StyleSheet.create({
  //...
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


## A. ไฟล์สมบูรณ์ App.js 

```jsx
  import React from 'react';
  import { View, Text, StyleSheet, SafeAreaView, Button} from 'react-native';

  export default function App() {
    return (
      <View style={styles.container}>
        <SafeAreaView style={styles.safeAreaTop}>
        </SafeAreaView>
        <SafeAreaView style={styles.safeAreaBottom}>
          <View style={styles.header}>
            <Text style={styles.headerText}>Header</Text>
          </View>
          <View style={styles.content}>
            <Text style={styles.contentText}>Content</Text>

            {/* เพิ่มปุ่มในส่วน content */}
            <Button title="ลงชื่อเข้าใช้" color={styles.signInButton.color} />
          </View>
          <View style={styles.footer}>
            <Text style={styles.footerText}>Footer</Text>
          </View>
        </SafeAreaView>
      </View>
    );
  };

  const styles = StyleSheet.create({
    // เพิ่ม Stylesheet สำหรับ SafeArea ด้านบน กำหนด flex เป็น 0 เพื่อไม่ให้กินพื้นที่ลงมาด้านล่าง
    safeAreaTop: {
      flex: 0,
      backgroundColor: 'red',
    },
    // เพิ่ม Stylesheet สำหรับ SafeArea ด้านล่าง กำหนด flex เป็น 1 เพื่อให้กินพื้นที่ด้านล่างทั้งหมด
    safeAreaBottom: {
      flex: 1,
      backgroundColor: 'green',
    },
    container: {
      flex: 1,
      flexDirection: 'column',
    },
    header: {
      flex: 1,
      backgroundColor: 'red',
      justifyContent: 'center',
      alignItems: 'center',
    },
    headerText: {
      fontSize: 24,
      color: 'white',
    },
    content: {
      flex: 18,
      backgroundColor: 'white',
      justifyContent: 'flex-start',
      alignItems: 'flex-start',
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
    signInButton: {
      color: 'orange'
    }
  });

  export default App;


```
