
# 15. บันทึกข้อมูลเมื่อมีการแสกน Barcode

## 1. สร้าง function ที่บันทึก barcode ลง database 

เปิดไฟล์ `services/db.service.js`

```js

// import function ในการสร้าง Async thynk ของ redux 
import { createAsyncThunk } from "@reduxjs/toolkit";

import * as SQLite from "expo-sqlite";
import { useState } from "react";

const dbName = 'barcodeDB.db';
const tableName = 'BarcodeHistory';
const db = SQLite.openDatabase(dbName);

export const initDB = () => {

    const [isDBReady, setIsDBReady] = useState(undefined)
    db.transaction(tx => {

        tx.executeSql(
            `CREATE TABLE IF NOT EXISTS ${tableName} (id INTEGER PRIMARY KEY NOT NULL, barcodeData text);`,
            [],
            () => {
                
                setIsDBReady(true);
            },
            (error) => {
                setIsDBReady(false);
                console.error(error);
            }
        );
    });

    return { isDBReady };

}

// สร้าง async function แบบ thunk เพื่อให้ทำงานแบบ async และกำหนดใช้งานเข้ากับ redux slice ได้
export const saveBarcode = createAsyncThunk(
    // ตั้งชื่อ 
    'db/saveBarcode',
    // กำหนด async function ที่จะรับค่า barcode data มา
    async (barcodeData: String) => {

        // สร้าง Promise object เพื่อควบคุมการทำงานแบบ Async ด้วยตัวเอง
        // เพราะ sqlite api ของ Expo มีแต่ callback function ไม่มีโหมดการทำงานแบบ async 
        return new Promise((resolve, reject) => {
            db.transaction(
                tx => {
                    // ใส่ข้อมูล barcode ลงไปใน database
                    tx.executeSql(
                        `INSERT INTO ${tableName} (barcodeData) values (?)`,
                        // กำหนดตัวแปรลงไปใน array เพื่อให้เอาไปแทนที่ ? ใน SQL statement
                        [barcodeData],
                       
                        () => {},
                        // ถ้าไม่สำเร็จ เรียกใช้ reject() function ถือเป็นการสิ้นสุดการทำงานของ promise 
                        // และเรียกว่าสถานะ rejected ใน slice's extra reducer
                        (error) => reject(error)
                    );

                    // Query ข้อมูลจาก Database 
                    tx.executeSql(
                        `SELECT * FROM ${tableName} ORDER BY id DESC`,
                        [],
                        (tx, { rows }) => {
                            // ถ้าสำเร็จ เรียกใช้ resolve() function ถือเป็นการสิ้นสุดการทำงานของ promise 
                            // และเรียกว่าสถานะ fulfiled ใน slice's extra reducer
                            // ในที่นี้เราจะส่งข้อมูลที่ query ได้กลับออกไปใน redux slice ในฐานะของ Action Payload 
                            resolve(rows._array);
                        },
                        // ถ้าไม่สำเร็จ เรียกใช้ reject() function ถือเป็นการสิ้นสุดการทำงานของ promise 
                        // และเรียกว่าสถานะ rejected ใน slice's extra reducer
                        (error) => reject(error)
                    );
    
            });
        })
    }
);

export default db;
```

## 2. กำหนด function ที่จะทำงานต่อผลของการเรียกใช้ Async Thunk 

เปิดไฟล์ `redux/barcodeDataSlice.js`

```js
import { createSlice } from '@reduxjs/toolkit'

// เรียกใช้ async thunk ที่สร้างไว้
import { saveBarcode } from '../services/db.service';

const initialState = {
    barcodeData: undefined,
    barcodeHistories: [],
}

const barcodeDataSlice = createSlice({
    name: 'barcodeData',
    initialState,
    reducers: {
        barcodeScanned: (state, action) => {
            console.log(`barcode data in slice: ${action.payload}`)
            state.barcodeData = action.payload;
        }
    },

    // กำหนดเคสที่จะทำงานเมื่อ async thunk 'saveBarcode' ทำงานเสร็จ ด้วย Builder ใน extraReducers ของ slice
    extraReducers: (builder) => {
        builder
            // เคสที่ saveBarcode ทำงานสมบูรณ์ ไม่มี rejected หรือ error 
            .addCase(saveBarcode.fulfilled, (state, action) => {
                console.log(`Save barcode data succeed, current barcode count: ${action.payload.length}`);
                state.barcodeHistories = action.payload;
            })
            // เคสที่ saveBarcode โดน rejected หรือ error 
            .addCase(saveBarcode.rejected, (state, action) => {
                console.error('Save barcode data failed: ' + action.payload)
            })
    }
});
export const { barcodeScanned } = barcodeDataSlice.actions

export default barcodeDataSlice.reducer
```


## 3. dispatch action ที่ได้จาก async thunk เข้าไปที่ redux

เปิดไฟล์ `pages/scan-page/ScanPage.js`


```jsx
import { StyleSheet, View } from 'react-native'
import React, { useEffect, useState } from 'react'
import { Box, HStack, Text } from 'native-base'
import { BarCodeScanner } from 'expo-barcode-scanner'
import { useDispatch } from 'react-redux'
import { barcodeScanned } from '../../redux/barcodeDataSlice'

// import async thunk `saveBarcode` เพื่อส่งข้อมูลไปบันทึกใน database
import { saveBarcode } from '../../services/db.service'

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

        // ส่งข้อมูล barcode ไปบันทึกใน database ผ่าน Async thunk
        const asyncAction = saveBarcode(data)
        dispatch(asyncAction);

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

ไปเป็นแบบนี้ 

```js
const mapDispatchToProps = dispatch => {
    return {
        barcodeScanned: (barcodeData) => actions.barcodeScanned(dispatch, barcodeData) // เห็นที่เราส่ง dispatch เข้าไปใน action function ไหม?
    }
}
```


## 5. ปรับหน้า HomePage ให้แสดงข้อมูลจาก `barcodeHistories` ของ Redux state  

เปิดไฟล์ `pages/home-page/HomePage.js`

```jsx
import { StyleSheet, View } from 'react-native'
import React, { useEffect } from 'react'
// import component ที่จำเป็น
import { Box, FlatList, HStack, Text, VStack } from 'native-base'
import { useSelector } from 'react-redux'

const HomePage = () => {

    // ยกเลิกการดึง barcode.barcodeData 
    // let barcodeData = useSelector(state => state.barcode.barcodeData)

    // ใช้ข้อมูลจาก barcode.barcodeHistories ในการแสดงรายการข้อมูลย้อนหลังแทน
    const barcodeHistories = useSelector(state => state.barcode.barcodeHistories)

    // แสดงข้อมูลใน barcodeHistories บน console
    console.log(barcodeHistories);

    // ปรับการแสดงข้อมูลบนหน้า home page ให้ตอบรับกับข้อมูลการแสกนย้อนหลัง
    return (
        <>
            {
                // เช็คว่ามีข้อมูลย้อนหลังของ barcode หรือไม่ ถ้ามีให้แสดงเป็น list
                barcodeHistories.length > 0 ? (
                    <FlatList
                    // กำหนด barcodeHistories เป็นข้อมูลของ FlatList
                        data={barcodeHistories}
                    // กำหนด function render ที่จะ return เป็น component ที่ต้องการแสดงข้อมูลของแต่ละ item ที่อยู่ใน data ของ FlatList
                    // ในที่นี้ function จะได้ object item มาใช้งานด้วย
                        renderItem={({ item }) => (
                            <Box borderBottomWidth="1" paddingLeft={5} paddingBottom={5} paddingTop={5}>
                                <VStack space={1}>
                                    <Text>id: {item.id}</Text>
                                    <Text>code: {item.barcodeData}</Text>
                                </VStack>
                            </Box>
                        )}
                    />
                    // ถ้าไม่มีให้แสดงเป็นข้อความ nothing here...
                ) : (
                    <Box padding="10">
                        <Text>Nothing here...</Text>
                    </Box >
                )
            }

        </>
    )
}

export default HomePage

const styles = StyleSheet.create({})
```

