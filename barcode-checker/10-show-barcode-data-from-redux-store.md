
# 11. แสดงข้อมูล Barcode ในหน้า HomePage จาก Redux store

## 1. ใช้ navigation ย้อนกลับไปที่ screen ก่อนหน้า หลัง scan barcode สำเร็จ

เปิดไฟล์ `pages/scan-page/ScanPage.js`

```jsx
// pages/scan-page/ScanPage.js

import { StyleSheet, View } from 'react-native'
import React, { useEffect, useState } from 'react'
import { Box, HStack, Text } from 'native-base'
import { BarCodeScanner } from 'expo-barcode-scanner'
import { useDispatch } from 'react-redux'
import { barcodeScanned } from '../../redux/barcodeDataSlice'

// เรียกใช้ navigation object ที่ถูกส่งผ่าน props
const ScanPage = ({ navigation }) => {

    const [hasPermission, setHasPermission] = useState(null);
    const [scanned, setScanned] = useState(false);

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

        const action = barcodeScanned(data);
        dispatch(action)

        // สั่งให้กลับไปที่ screen ก่อนหน้า
        navigation.goBack()
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

## 2. ทำการแสดง Barcode ที่แสกนได้บน Home page

เปิดไฟล์ `pages/home-page/HomePage.js`



```jsx
import { StyleSheet, View } from 'react-native'
import React, { useEffect } from 'react'

import { Box, HStack, Text } from 'native-base'
// import useSelector เพื่อดึง State จาก Redux store มาใช้งาน
import { useSelector } from 'react-redux'

const HomePage = () => {

    // ดึงข้อมูลที่ชื่อ barcodeData จาก Reducer ที่เราตั้งชื่อว่า barcode ใน Redux store
    let barcodeData = useSelector(state => state.barcode.barcodeData)

    // แสดงข้อมูล
    console.log(barcodeData);

    return (
        <>
            <Box padding="10">
                {/* เช็คข้อมูลในตัวแปร barcodeData
                    ถ้ามีข้อมูลให้แสดงขึ้นมา ถ้าไม่มีให้แสดงว่า nothing here...
                */}
                {barcodeData ?
                    <Text>Barcode Data: {barcodeData}</Text>
                    : <Text>Nothing here...</Text>
                }
            </Box>
        </>
    )
}

export default HomePage

const styles = StyleSheet.create({})
```

