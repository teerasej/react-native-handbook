
# 10. ส่งข้อมูล Barcode ได้เข้า Redux store

## 1. สร้าง Action ที่ใช้ในระบบ


```js
// redux/barcodeDataSlice.js
import { createSlice } from '@reduxjs/toolkit'

const initialState = {
    barcodeData: undefined
}

const barcodeDataSlice = createSlice({
    name: 'barcodeData',
    initialState,

    // กำหนด function ชื่อ barcodeScanned เป็น reducer 
    //   - โดยที่ตัว function จะทำงานเมื่อมีการเรียกใช้จากภายใน component
    //   - ทุกครั้งที่ function reducer ทำงาน จะได้รับ state object ล่าสุด และ action ที่ส่งมาจาก component เสมอ
    reducers: {
        barcodeScanned: (state, action) => {
            console.log(`barcode data in slice: ${action.payload}`)
            // ดึงค่าที่ส่งมากับ action ใส่ลงไปใน state
            state.barcodeData = action.payload
        }
    }
});

// export reducer สำหรับไปเรียกใช้ที่ component ที่ต้องการ
export const { barcodeScanned } = barcodeDataSlice.actions

export default barcodeDataSlice.reducer
```

## 2. ส่งข้อมูล barcode ที่แสกนได้ไปที่ redux store

เปิดไฟล์ `pages/scan-page/ScanPage.js`


```jsx
// pages/scan-page/ScanPage.js
import { StyleSheet, View } from 'react-native'
import React, { useEffect, useState } from 'react'
import { Box, HStack, Text } from 'native-base'

import { BarCodeScanner } from 'expo-barcode-scanner'

// เรียกใช้ useDispatch hook
import { useDispatch } from 'react-redux'
// เรียกใช้ action function ในการสร้าง action object เพื่อส่งให้กับ redux 
import { barcodeScanned } from '../../redux/barcodeDataSlice'

const ScanPage = () => {

    const [hasPermission, setHasPermission] = useState(null);
    const [scanned, setScanned] = useState(false);

    // สร้าง dispatch function 
    const dispatch = useDispatch();  

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

    const handleBarCodeScanned = ({ type, data }) => {
        setScanned(true);
        alert(`Bar code with type ${type} and data ${data} has been scanned!`);

        // ใส่ข้อมูลบาร์โค้ดที่แสกนได้ให้กับ barcodeScanned action 
        const action = barcodeScanned(data);

        // ส่ง action ไปที่ redux store ผ่าน dispatch function
        dispatch(action)
    };

    return (
        <>
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
