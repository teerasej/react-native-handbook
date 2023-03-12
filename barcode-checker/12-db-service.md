
# 13. สร้าง Service เพื่อจัดการส่วนของ Database

## 1. สร้างไฟล์ db.service.js

สร้างไฟล์ `services/db.service.js`

```js
import * as SQLite from "expo-sqlite";

// กำหนดชื่อไฟล์ database และ ชื่อ Table
const dbName = 'barcodeDB.db';
const tableName = 'BarcodeHistory';

// ใช้ SQLite ของ Expo ในการเปิดการเชื่อมต่อไปที่ไฟล์ database
const db = SQLite.openDatabase(dbName);

export default db
```

## 2. สร้าง `initDB()` hook function

ในที่นี้เราจะสร้าง hook function เพื่อเตรียม table ให้พร้อมใช้งานในแอพ

```js
// services/db.service.js

import * as SQLite from "expo-sqlite";
import { useState } from "react";

const dbName = 'barcodeDB.db';
const tableName = 'BarcodeHistory';
const db = SQLite.openDatabase(dbName);

export const initDB = () => {

    // สร้าง State variable ที่แสดงสถานะของ database ที่พร้อมสำหรับการใช้งาน
    const [isDBReady, setIsDBReady] = useState(undefined)

    // สั่งรัน transaction ไปที่ database
    db.transaction(tx => {

        // Execute SQL ในการสร้าง table สำหรับเก็บข้อมูลบาร์โค้ดที่แสกนได้
        tx.executeSql(
            `CREATE TABLE IF NOT EXISTS ${tableName} (id INTEGER PRIMARY KEY NOT NULL, barcodeData text);`,
            [],
            () => {
                
                // กำหนดค่ายืนยันว่า Database และ table พร้อมใช้งานแล้ว
                setIsDBReady(true);
            },
            (error) => {
                // กำหนดค่าว่่า database ไม่พร้อมใช้งาน
                setIsDBReady(false);
                console.error(error);
            }
        );
    });

    // ส่ง State variable ออกไปให้กับ component ที่เรียกใช้ initDB() hook ของเรา
    return { isDBReady };

}

export default db;
```



