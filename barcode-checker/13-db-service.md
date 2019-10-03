
# 13. สร้าง Service เพื่อจัดการส่วนของ Database

## 1. สร้างไฟล์ db.service.js

สร้างไฟล์ `services/db.service.js`

```js
import * as SQLite from 'expo-sqlite';

// กำหนดชื่อไฟล์ database และ ชื่อ Table
const dbName = 'barcodeDB.db';
const tableName = 'BarcodeHistory';

// ใช้ SQLite ของ Expo ในการเปิดการเชื่อมต่อไปที่ไฟล์ database
const db = SQLite.openDatabase(dbName);

export default class DBService {

} 
```

## 2. สร้าง method `init()` เพื่อสร้าง table สำหรับการใช้งาน

สังเกตว่า เรา return เป็น Promise() object เพื่อสร้างการทำงานแบบ Asynchronous และทำให้ใช้งาน keyword `async` และ `await` ได้

```js
init() {

    // Promise จะรับ function เข้ามา 2 ตัว
    // resolve() สำหรับกรณีการทำงานเสร็จสมบูรณ์
    // reject() สำหรับกรณีที่ไม่สมบูรณ์
    return new Promise((resolve, reject) => {
        db.transaction(tx => {
            tx.executeSql(
                `CREATE TABLE IF NOT EXISTS ${tableName} (id INTEGER PRIMARY KEY NOT NULL, barcodeData text);`,
                [],
                () => {
                    console.log('db ready');

                    // สั่ง reolve เพื่อยืนยันการทำงานที่สมบูรณ์
                    resolve();
                }
            );
        });
    });

}
```

## A. ไฟล์เต็ม `services/db.service.js`

```js
import * as SQLite from 'expo-sqlite';

const dbName = 'barcodeDB.db';
const tableName = 'BarcodeHistory';
const db = SQLite.openDatabase(dbName);

export default class DBService {

    init() {

        return new Promise((resolve, reject) => {
            db.transaction(tx => {
                tx.executeSql(
                    `CREATE TABLE IF NOT EXISTS ${tableName} (id INTEGER PRIMARY KEY NOT NULL, barcodeData text);`,
                    [],
                    () => {
                        console.log('db ready');
                        resolve();
                    }
                );
            });
        });

    }

} 
```