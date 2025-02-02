
# Event ใน Function component 

- การใช้งาน User Interface จะทำให้เกิด event และเราสามารถนำ function มารับ event พวกนี้ได้

## สร้าง Function สำหรับใช้กับ Event 

เปิดไฟล์ `app\index.tsx` และเขียน function ชื่อ `showAlert`

```jsx
// app/index.tsx

import React from "react";

// import Alert มาจาก react-native
import { View, Alert } from "react-native";
import Hello from "./Hello";

// import Button และ ButtonText มาจาก ui/button
import { Button, ButtonText } from "@/components/ui/button";

export default function Index() {

  // สร้าง function ชื่อ showAlert ที่ใช้ Alert แสดงข้อความ
  const showAlert = () => {
    Alert.alert("Welcome", "Hello, Nextflow!");
  };

  return <View style={{
    flex: 1,
    justifyContent: "center",
    alignItems: "center"
  }}>
    <Hello name="Nextflow" />

    {/* สร้าง Button ที่เมื่อกดจะเรียก function showAlert */}
    {/* สามารถใช้ snippet `gs-button` */}
    {/* และกำหนด event handler โดยใช้การกำหนด function ลงไปใน prop 'onPress' [onPress={showAlert}]*/}
    <Button action={"primary"} variant={"solid"} size={"lg"} onPress={showAlert}>
      <ButtonText>
         Show Alert
      </ButtonText>
    </Button>
  </View>;
}

```

2. บันทึกไฟล์
3. รันโปรเจคด้วยคำสั่ง `npx expo start` และเปิดโปรแกรม Expo Go บนมือถือเพื่อดูผลลัพธ์