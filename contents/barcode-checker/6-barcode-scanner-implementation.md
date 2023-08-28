
# 6. สร้างระบบ Scan Barcode

## อ้างอิง

> [Expo Barcode Scanner](https://docs.expo.io/versions/latest/sdk/bar-code-scanner/)

## โปรเจคเริ่มต้น

สามารถ[ดาวน์โหลด zip file ของโปรเจคจาก Git repo](https://github.com/teerasej/nextflow-react-native-barcode-checker-2/tree/start-barcode-implementation-page) มาทำเฉพาะขั้นตอนนี้ได้

## 1. ติดตั้ง Plugin 

รันคำสั่งใน Terminal

```bash
npx expo install expo-barcode-scanner
```

## 2. กำหนด permission ใน `app.json`

สำหรับ Barcode scanner จำเป็นต้องมีการขอ permission ซึ่งสามารถทำได้ผ่าน `app.json` 

```json
{
  "expo": {
    "plugins": [
      [
        "expo-barcode-scanner",
        {
          "cameraPermission": "Allow $(PRODUCT_NAME) to access camera."
        }
      ]
    ]
  }
}
```

## 3. สร้างกลไกเช็ค Permission

เปิดไฟล์ `pages/scan-page/ScanPage.js`

```jsx
// pages/scan-page/ScanPage.js

import { StyleSheet, View } from 'react-native'

// Import React hook 
// 1. useEffect เพื่อรันโค้ดหลังจากที่ component render ขึ้นมาบนหน้าจอแล้ว
// 2. useState เพื่อใช้งานกลไก State
import React, { useEffect, useState } from 'react'

import { Box, HStack, Text } from 'native-base'

// Import BarcodeScanner
import { BarCodeScanner } from 'expo-barcode-scanner'

const ScanPage = () => {

    // สร้าง State variable เก็บค่าผลลัพธ์การขอ permission ในการเปิดกล้องแสกน
    const [hasPermission, setHasPermission] = useState(null);

    // ใช้ useEffect เพื่อรัน function ด้านใน หลังจาก component render ขึ้นมาบนหน้าจอแล้ว
    useEffect(() => {

        console.log('checking permission')

        // สร้าง async function ในการเช็คการขอ permission barcode scanner
        const getBarCodeScannerPermissions = async () => {
            const result = await BarCodeScanner.requestPermissionsAsync();
            setHasPermission(result.granted);
        };
        
        // เรียกใช้ Async function
        getBarCodeScannerPermissions();
    }, []);

    // เช็ค permission เพื่อแสดงข้อความบนหน้าจอที่แตกต่างกัน
    if (hasPermission === null) {
        console.log('requesting permission')
        return <Text>Requesting for camera permission</Text>;
    }
    if (hasPermission === false) {
        // ถ้าไม่มี permission ก็จะขึ้นข้อความแบบนี้
        console.log('no permission detected')
        return <Text>No access to camera</Text>;
    }

    return (
        <>
            {/* แสดงส่วนกล้อง Scan */}
            <BarCodeScanner 
                style={StyleSheet.absoluteFillObject}
            />
        </>
    )
}

export default ScanPage

const styles = StyleSheet.create({})
```


## 4. สร้าง function รับค่าที่เป็นข้อมูล Barcode หลังจาก scan ได้

```jsx
// pages/scan-page/ScanPage.js

import { StyleSheet, View } from 'react-native'
import React, { useEffect, useState } from 'react'
import { Box, HStack, Text } from 'native-base'

import { BarCodeScanner } from 'expo-barcode-scanner'

const ScanPage = () => {

    const [hasPermission, setHasPermission] = useState(null);

    // สร้าง State variable สำหรับเก็บค่ายืนยันว่า Scan สำเร็จไปหรือยัง
    const [scanned, setScanned] = useState(false);

    useEffect(() => {

        console.log('checking permission')

        const getBarCodeScannerPermissions = async () => {
            const result = await BarCodeScanner.requestPermissionsAsync();
            console.log(`Permission: ${result.granted}`)
            setHasPermission(result.granted);
        };

        getBarCodeScannerPermissions();
    }, []);

    if (hasPermission === null) {
        console.log('requesting permission')
        return <Text>Requesting for camera permission</Text>;
    }
    if (hasPermission === false) {
        console.log('no permission detected')
        return <Text>No access to camera</Text>;
    }

    // กำหนด function ที่จะรับข้อมูลการแสกน
    const handleBarCodeScanned = ({ type, data }) => {

        // กำหนด State scanned เป็น true เพื่อยืนยันว่า Scan เสร็จแล้ว จะไม่ต้องอ่านค่า barcode อีก
        setScanned(true);

        // alert popup ประเภท และข้อมูลบาร์โค้ดที่แสกนได้
        alert(`Bar code with type ${type} and data ${data} has been scanned!`);
    };

    return (
        <>
            {/* 
                ใส่ function ให้กับ event onBarCodeScanned  
                โดยเทียบค่า state variable ชื่อ scanned ว่าถ้าแสกนไปแล้วก็ไม่ต้องเรียกใช้ function รับข้อมูลการแสกน
            */}
            <BarCodeScanner 
                onBarCodeScanned={scanned ? undefined : handleBarCodeScanned}
                style={StyleSheet.absoluteFillObject}
            />
        </>
    )
}

export default ScanPage

const styles = StyleSheet.create({})
```
