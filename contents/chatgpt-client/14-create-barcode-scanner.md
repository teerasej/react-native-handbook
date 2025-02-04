
# สร้าง Component ที่เกี่ยวกับการ Scan Barcode

## 1. สร้าง index.tsx สำหรับแสดงหน้า Barcode Page 

```tsx
// app/barcode/index.tsx

import { Button, ButtonText, ButtonSpinner, ButtonIcon } from '@/components/ui/button';

import React from 'react';
import { Stack, useRouter } from 'expo-router';
import { Box } from '@/components/ui/box';
export default function index() {


  const handleStartScan = () => {
   
  }

  return <>
    <Stack.Screen options={{
      title: "Barcode Page"
    }}/>
    
      <Box className="flex-1 p-2">
        <Button onPress={handleStartScan}>
          <ButtonText>Scan</ButtonText>
        </Button>
      </Box>
  </>;
}
```

## 2. สร้างปุ่มจากหน้าแรก มายังหน้า Barcode Page

สามารถอ้างอิงการวางโครงสร้างของ URL ได้จาก https://docs.expo.dev/router/create-pages/

```tsx
// app/index.tsx

import React from "react";
import { Link } from "expo-router";
import { VStack } from "@/components/ui/vstack";
import { Stack } from "expo-router";
import { Button, ButtonText } from "@/components/ui/button";

export default function Home() {

  // Link ที่ 2 ที่เปิดไปหน้า Barcode Page นั้นสามารถใช้ match กับ /barcode/index.tsx ได้ จากคู่มือการใช้งานของ expo-router https://docs.expo.dev/router/create-pages/
  return (
    <>
      <Stack.Screen
        options={{
          title: "Home"
        }}
      />

      <VStack className="flex-1 p-2" space="md">
        <Link href="/chat" asChild>
          <Button>
            <ButtonText>Ask AI</ButtonText>
          </Button>
        </Link>

        <Link href="/barcode" asChild>
          <Button>
            <ButtonText>Barcode</ButtonText>
          </Button>
        </Link>
      </VStack>

    </>
  );
}

```

## 3. สร้างหน้าขอ Permission และเปิดกล้อง

สร้างไฟล์ `app/barcode/scan.tsx` ดังนี้

```tsx
// app/barcode/scan.tsx

import { Box } from '@/components/ui/box';
import { Text } from '@/components/ui/text';
import React, { useState } from 'react';
import { Stack } from 'expo-router';

export default function ScanPage() {

    return (
        <Box className='flex-1'>
            <Stack.Screen options={{
                title: "Scan"
            }} />

            <Text>Scanner Page</Text>
        </Box>
    );
}
```

## 4. กำหนดให้ปุ่มในหน้า Barcode Page เปิดไปยังหน้า Scan ได้

```tsx
// app/barcode/index.tsx

import { Button, ButtonText, ButtonSpinner, ButtonIcon } from '@/components/ui/button';

import React from 'react';
import { Stack, useRouter } from 'expo-router';
import { Box } from '@/components/ui/box';
import { Text } from '@/components/ui/text';

export default function index() {

  // เรียกใช้ router จาก expo-router
  const router = useRouter()

  const handleStartScan = () => {
    // ใช้ router.navigate เพื่อเปลี่ยนหน้าไปยัง /barcode/scan
    router.push('/barcode/scan')
  }

  return <>
    <Stack.Screen options={{
      title: "Barcode Scanner"
    }}/>
    
      <Box className="flex-1 p-2">
        <Button onPress={handleStartScan}>
          <ButtonText>Scan</ButtonText>
        </Button>
      </Box>
  </>;
}
```

## 5. เสร็จสิ้นการวาง Layout

บันทึกไฟล์ และทดสอบการเปิดมาดูหน้า Barcode Page และกดปุ่ม Scan จะเปิดหน้า Scan Page ขึ้นมา