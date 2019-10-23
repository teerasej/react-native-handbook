
# บันทึกข้อมูล

> ดู Document เพิ่มเติมได้ที่ [Expo SQLite](https://docs.expo.io/versions/latest/sdk/sqlite/)

- [Download Starter Project](https://www.dropbox.com/s/qx0w1pilke12vg7/barcode-checker-starter-only-db.zip?dl=0)
- [Download Finish Project](https://www.dropbox.com/s/9kuq8vki75uupx7/barcode-checker-finish-real-scan.zip?dl=0)


## 1. กำหนดชื่อไฟล์ database และเปิดการเชื่อมต่อกับไฟล์

เปิดไฟล์ `services/db.service.js`

```js
import * as SQLite from 'expo-sqlite';

const dbName = 'barcodeDB.db';
const tableName = 'BarcodeHistory';
const db = SQLite.openDatabase(dbName);
```

## 2. เขียน init() method เพื่อสร้าง table บน Database

```js
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
```

## 3. เขียน saveBarcode() method เพื่อบันทึก และโหลดข้อมูลจาก Database

```js
saveBarcode(barcodeData: String) {
        return new Promise((resolve, reject) => {
            db.transaction(
                tx => {
                    tx.executeSql(`INSERT INTO ${tableName} (barcodeData) values (?)`, [barcodeData]);
                    tx.executeSql(
                        `SELECT * FROM ${tableName}`,
                        [],
                        (tx, { rows }) => {
                            // console.log(JSON.stringify(rows));
                            resolve([...rows._array]);
                        }
                    );
                }
            );
        })
    }
```

## 4. เขียน loadBarcode() method เพื่อโหลดข้อมูลจาก Database

```js
loadBarcode() {
        return new Promise((resolve, reject) => {
            db.transaction(
                tx => {
                    tx.executeSql(
                        `SELECT * FROM ${tableName}`,
                        [],
                        (tx, { rows }) => {
                            // console.log(JSON.stringify(rows));
                            resolve([...rows._array]);
                        }
                    );
                }
            );
        })
    }
```

## ไฟล์เต็ม services/db.service.js

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

    saveBarcode(barcodeData: String) {
        return new Promise((resolve, reject) => {
            db.transaction(
                tx => {
                    tx.executeSql(`INSERT INTO ${tableName} (barcodeData) values (?)`, [barcodeData]);
                    tx.executeSql(
                        `SELECT * FROM ${tableName}`,
                        [],
                        (tx, { rows }) => {
                            // console.log(JSON.stringify(rows));
                            resolve([...rows._array]);
                        }
                    );
                }
            );
        })
    }

    loadBarcode() {
        return new Promise((resolve, reject) => {
            db.transaction(
                tx => {
                    tx.executeSql(
                        `SELECT * FROM ${tableName}`,
                        [],
                        (tx, { rows }) => {
                            // console.log(JSON.stringify(rows));
                            resolve([...rows._array]);
                        }
                    );
                }
            );
        })
    }

    removeBarcode(barcodeId) {
        return new Promise((resolve, reject) => {
            db.transaction(
                tx => {
                    const sql = `DELETE FROM ${tableName} WHERE id = ?`;
                    console.log(sql, 'with barcode Id:', barcodeId)
                    tx.executeSql(
                        sql,
                        [barcodeId],
                        (tx, { rowsAffected, rows }) => {
                            console.log(`Affected: ${rowsAffected}`);
                            resolve([...rows._array]);
                        }
                    );
                }
            );
        })
    }

    

}
```