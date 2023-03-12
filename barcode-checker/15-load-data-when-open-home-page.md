
# 16. โหลดข้อมูลที่มีอยู่ใน SQLite Database เมื่อเปิดหน้า Home Page

## 1. สร้าง Async thunk สำหรับโหลดข้อมูล barcode ที่เก็บไว้ ใน DB Service 


เปิดไฟล์ `services/db.service.js`

```js
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

export const saveBarcode = createAsyncThunk(
    'db/saveBarcode',
    async (barcodeData: String) => {
        return new Promise((resolve, reject) => {
            db.transaction(
                tx => {
                    tx.executeSql(
                        `INSERT INTO ${tableName} (barcodeData) values (?)`,
                        [barcodeData],
                        () => {},
                        (error) => reject(error)
                    );

                    tx.executeSql(
                        `SELECT * FROM ${tableName} ORDER BY id DESC`,
                        [],
                        (tx, { rows }) => {
                            resolve(rows._array);
                        },
                        (error) => reject(error)
                    );
    
            });
        })
    }
);

// สร้าง async function แบบ thunk เพื่อให้ทำงานแบบ async และกำหนดใช้งานเข้ากับ redux slice ได้
export const loadBarcodeHistories = createAsyncThunk(
    // ตั้งชื่อ 
    'db/loadBarcodeHistories',
    // กำหนด async function 
    async () => {

        // สร้าง Promise object เพื่อควบคุมการทำงานแบบ Async ด้วยตัวเอง
        // เพราะ sqlite api ของ Expo มีแต่ callback function ไม่มีโหมดการทำงานแบบ async 
        return new Promise((resolve, reject) => {
            db.transaction(
                tx => {
                    
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

## 2. อัพเดต Slice เพื่อรับผลการเรียกใช้ Async thunk 


เปิดไฟล์ `redux/barcodeDataSlice.js`


```js
import { createSlice } from '@reduxjs/toolkit'

// เรียกใช้ async thunk ที่สร้างไว้
import { loadBarcodeHistories, saveBarcode } from '../services/db.service';

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
            .addCase(saveBarcode.fulfilled, (state, action) => {
                console.log(`Save barcode data succeed, current barcode count: ${action.payload.length}`);
                state.barcodeHistories = action.payload;
            })
            .addCase(saveBarcode.rejected, (state, action) => {
                console.error('Save barcode data failed: ' + action.payload)
            })

            // เคสที่ loadBarcodeHistories ทำงานสมบูรณ์ ไม่มี rejected หรือ error 
            .addCase(loadBarcodeHistories.fulfilled, (state, action) => {
                console.log(`Load barcode data succeed, current barcode count: ${action.payload.length}`);
                state.barcodeHistories = action.payload;
            })
            // เคสที่ loadBarcodeHistories โดน rejected หรือ error 
            .addCase(loadBarcodeHistories.rejected, (state, action) => {
                console.error('Load barcode data failed: ' + action.payload)
            })
            
    }
});
export const { barcodeScanned } = barcodeDataSlice.actions

export default barcodeDataSlice.reducer
```

## 3. สั่งโหลดข้อมูล barcode มาแสดงหลังจากหน้า HomePage ถูกโหลด

เปิดไฟล์ `pages/home-page/HomePage.js`

```jsx
import { StyleSheet, View } from 'react-native'
import React, { useEffect } from 'react'
import { Box, FlatList, HStack, Text, VStack } from 'native-base'

// เรียกใช้งาน useDispatch
import { useDispatch, useSelector } from 'react-redux'

import { loadBarcodeHistories } from '../../services/db.service'

const HomePage = () => {

    const barcodeHistories = useSelector(state => state.barcode.barcodeHistories)

    // เรียกใช้ dispatch สำหรับการใช้ thunk
    const dispatch = useDispatch();

    // ทำการ dispatch action ของ thunk เพื่อโหลดข้อมูลจาก SQLite ตอนเริ่มแสดงหน้า HomePage
    useEffect(() => {
      dispatch(loadBarcodeHistories())

    //   ถ้า dispatch เป็นตัวเดิม กลไก useEffect จะไม่ทำงานอีก
    }, [dispatch])
    

    console.log(barcodeHistories);

    return (
        <>
            {
                barcodeHistories.length > 0 ? (
                    <FlatList
                        data={barcodeHistories}
                        renderItem={({ item }) => (
                            <Box borderBottomWidth="1" paddingLeft={5} paddingBottom={5} paddingTop={5}>
                                <VStack space={1}>
                                    <Text>id: {item.id}</Text>
                                    <Text>code: {item.barcodeData}</Text>
                                </VStack>
                            </Box>
                        )}
                    />
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