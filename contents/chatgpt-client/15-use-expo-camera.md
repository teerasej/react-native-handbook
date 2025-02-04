
# เรียกใช้งาน Expo Camera เพื่อสร้าง UI เพื่อ Scan Barcode

> ต้องทำ[เงื่อนไขตามลำดับให้ครบ](./13-install-expo-camera.md) ถึงจะทำส่วนนี้ต่อได้

## 1. ใช้ react hook `useCameraPermissions` เพื่อเช็ค permission

1. เปิดมาที่หน้า `app/barcode/scan.tsx` ควรจะมีโค้ดประมาณนี้

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

2. สร้างกลไกการขอ permission โดยใช้ react hook `useCameraPermissions` โดยเราจะเรียกใช้งาน react hook นี้ในหน้า `app/barcode/scan.tsx`

```tsx
// app/barcode/scan.tsx

import { Box } from '@/components/ui/box';
import { Text } from '@/components/ui/text';
import React from 'react';
import { useCameraPermissions } from 'expo-camera';
import { Button, ButtonText } from '@/components/ui/button';
import { Stack } from 'expo-router';

export default function ScanPage() {

    // ใช้ react hook `useCameraPermissions` เพื่อเช็ค permission
    const [permission, requestPermission] = useCameraPermissions();

    // ถ้า permission เป็น null หรือ undefined ให้แสดงข้อความว่ากำลังขอ permission
    if (!permission) {
        return (
            <Box className="flex-1 justify-center align-middle">
                <Text>Requesting for camera permission...</Text>
            </Box>
        )
    }

    // ถ้า permission ไม่ได้รับการอนุญาตให้แสดงข้อความว่าต้องขอ permission ก่อน และแสดงปุ่ม Request permission
    // requestPermission คือ function ที่ใช้ขอ permission ที่ได้มาจาก react hook `useCameraPermissions`
    if (permission.granted === false) {
        return (
            <Box className="flex-1 justify-center align-middle">
                <Text>We need your permission to show the camera</Text>
                <Button onPress={requestPermission}>
                    <ButtonText>Request permission</ButtonText>
                </Button>
            </Box>
        )
    }

    return (
        <Box className='flex-1'>
            <Stack.Screen options={{
                title: "Scan"
            }} />

            <Text>Camera Permission Granted</Text>
        </Box>
    );
}
```

3. บันทึกไฟล์ และทดสอบว่าหน้า Scan ของเรามีการขอ permission หรือไม่ ถ้าไม่มีจะมีปุ่ม Request permission ให้กดเพื่อขอ permission
4. กดปุ่ม Request permission จะมีการขอ permission จากเครื่องของเรา ถ้ากดอนุญาต เราควรจะเห็นคำว่า **Camera Permission Granted** บนจอ

## 2. ใช้ CameraView Component 

1. ในหน้า `app/barcode/scan.tsx` ให้เรียกใช้งาน `CameraView` component ของ expo-camera

```tsx

import { Box } from '@/components/ui/box';
import { Text } from '@/components/ui/text';
import React from 'react';
import { CameraView, useCameraPermissions } from 'expo-camera';
import { Button, ButtonText } from '@/components/ui/button';
import { Stack } from 'expo-router';

export default function ScanPage() {

    const [permission, requestPermission] = useCameraPermissions();


    if (!permission) {
        return (
            <Box className="flex-1 justify-center align-middle">
                <Text>Requesting for camera permission...</Text>
            </Box>
        )
    }

    if (permission.granted === false) {
        return (
            <Box className="flex-1 justify-center align-middle">
                <Text>We need your permission to show the camera</Text>
                <Button onPress={requestPermission}>
                    <ButtonText>Request permission</ButtonText>
                </Button>
            </Box>
        )
    }

    // เพิ่ม CameraView component ลงไปในหน้า ScanPage โดยกำหนด style ให้เต็มจอ
    return (
        <Box className='flex-1'>
            <Stack.Screen options={{
                title: "Scan"
            }} />

            <CameraView 
                style={{ flex: 1 }}
                >

            </CameraView>
        </Box>
    );
}
```

2. บันทึกไฟล์ และทดสอบว่าหน้า Scan ของเรามีการแสดง CameraView หรือไม่ ถ้ามีกล้องของเราควรจะเปิดขึ้นมา

## 3. กำหนดให้ CameraView สามารถแสกนและส่งค่า Barcode กลับมาได้

1. ในหน้า `app/barcode/scan.tsx` ให้เพิ่ม props `barcodeScannerSettings` โดยกำหนดค่าตามโค้ดด้านล่าง เพื่อให้ CameraView สามารถแสกน barcode ที่กำหนดได้
2. ให้เพิ่ม props `onBarCodeScanned` ให้กับ CameraView โดยเราจะสร้าง function `handleBarCodeScanned` ที่จะรับค่า barcode ที่ส่งกลับมา

```tsx

import { Box } from '@/components/ui/box';
import { Text } from '@/components/ui/text';
import React from 'react';
import { BarcodeScanningResult, CameraView, useCameraPermissions } from 'expo-camera';
import { Button, ButtonText } from '@/components/ui/button';
import { Stack } from 'expo-router';

export default function ScanPage() {

    const [permission, requestPermission] = useCameraPermissions();

    // สร้าง function handleBarcodeScanned ที่รับค่า BarcodeScanningResult ที่ส่งกลับมาจาก CameraView
    const handleBarcodeScanned = (result:BarcodeScanningResult) => {
        
        // แสดงค่า barcode ที่ส่งกลับมา
        console.log(result.data)
    }

    if (!permission) {
        return (
            <Box className="flex-1 justify-center align-middle">
                <Text>Requesting for camera permission...</Text>
            </Box>
        )
    }

    if (permission.granted === false) {
        return (
            <Box className="flex-1 justify-center align-middle">
                <Text>We need your permission to show the camera</Text>
                <Button onPress={requestPermission}>
                    <ButtonText>Request permission</ButtonText>
                </Button>
            </Box>
        )
    }

    // เพิ่ม onBarcodeScanned ให้กับ CameraView โดยส่ง function handleBarcodeScanned ไป
    return (
        <Box className='flex-1'>
            <Stack.Screen options={{
                title: "Scan"
            }} />

            <CameraView 
                style={{ flex: 1 }}
                barcodeScannerSettings={{
                   barcodeTypes:['code128','ean13','ean8','qr']
                }}
                onBarcodeScanned={handleBarcodeScanned}
                >

            </CameraView>
        </Box>
    );
}
```

3. บันทึกไฟล์ และทดสอบว่าหน้า Scan ของเรามีการแสดง CameraView และสามารถสแกน barcode ได้หรือไม่ ถ้าสแกนได้ ค่า barcode ที่สแกนจะแสดงออกมาที่ console ของเรา