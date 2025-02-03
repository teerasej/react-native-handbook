
# 4. สร้าง UI หน้า HomePage

## 1. เตรียมไฟล์หน้า Home

เปิดไฟล์ `app/index.tsx` และสร้าง UI ตามด้านล่าง

```jsx
// app/index.tsx

import React from "react";
import { Box } from "@/components/ui/box";
import { ScrollView } from "react-native";
import { Text } from "@/components/ui/text";

import { Link } from "expo-router";


export default function Home() {
  return (
    <Box className="flex-1 bg-white h-[100vh]">
      <ScrollView
        style={{ height: "100%" }}
        contentContainerStyle={{ flexGrow: 1 }}
      >
        
      </ScrollView>
    </Box>
  );
}
```

บันทึกไฟล์และรีเฟรชหน้าเว็บ จะเห็นหน้า Home ว่างเปล่า

## 2. วาง Layout และ ช่องพิมพ์ข้อความ

เพิ่ม UI ของ Chat box  ในไฟล์ `app/index.tsx`

```jsx
// app/index.tsx

import React from "react";
import { Box } from "@/components/ui/box";
import { SafeAreaView, ScrollView } from "react-native";
import { Text } from "@/components/ui/text";

import { Link } from "expo-router";
import { Input, InputField } from "@/components/ui/input";
import { HStack } from "@/components/ui/hstack";
import { Button, ButtonText } from "@/components/ui/button";


export default function Home() {
  return (
    <SafeAreaView style={{ flex: 1 }}>
      <Box className="flex-1 bg-white h-[100vh] p-4">
        {/* จัดเรียง component ในแนวนอน */}
        <HStack space="md" >
        {/* Input box ที่กำหนดพื้นที่เต็มส่วน */}
          <Input style={{flex:1}}>
            <InputField placeholder="Enter text" />
          </Input>
        {/* ปุ่มส่งข้อความ */}
          <Button>
            <ButtonText>Send</ButtonText>
          </Button>
        </HStack>
      </Box>
    </SafeAreaView>
  );
}
```

บันทึกไฟล์และรีเฟรชหน้าเว็บ จะเห็นหน้า Home มีช่องพิมพ์ข้อความและปุ่มส่งข้อความ

## 3. ใช้ Button Icon 

เพิ่ม Icon ในปุ่มส่งข้อความ

```jsx
// app/index.tsx

import React from "react";
import { Box } from "@/components/ui/box";
import { SafeAreaView, ScrollView } from "react-native";
import { Text } from "@/components/ui/text";

import { Link } from "expo-router";
import { Input, InputField } from "@/components/ui/input";
import { HStack } from "@/components/ui/hstack";

// import ButtonIcon
import { Button, ButtonIcon, ButtonText } from "@/components/ui/button";

// import Icon
import { ChevronRightIcon } from '@/components/ui/icon'


export default function Home() {
  return (
    <SafeAreaView style={{ flex: 1 }}>
      <Box className="flex-1 bg-white h-[100vh] p-4">
        <HStack space="md" >
          <Input style={{ flex: 1 }}>
            <InputField placeholder="Enter text" />
          </Input>
          <Button>
            {/* ใช้ Icon ในปุ่ม */}
            <ButtonIcon as={ChevronRightIcon} />
          </Button>
        </HStack>
      </Box>
    </SafeAreaView>
  );
}
```

บันทึกไฟล์และรีเฟรชหน้าเว็บ จะเห็น Icon ในปุ่มส่งข้อความ
