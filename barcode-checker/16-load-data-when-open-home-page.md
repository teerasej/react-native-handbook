
# 16. โหลดข้อมูลเก่า เมื่อเปิดหน้า Home Page

## 1. สร้าง method `loadBarcode()` ใน DB Service 

เพื่อใช้เรียกข้อมูลจาก module อื่นๆ 

เปิดไฟล์ `services/db.service.js`

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
                            resolve(rows._array);
                        }
                    );
                }
            );
        })
    }
```

## 2. สร้าง action function `requestBarcodeData` 


เปิดไฟล์ `redux/actions.js`

และสร้าง function `requestBarcodeData` ที่สามารถ dispatch action ด้วยตัวเอง

```js
const requestBarcodeData = async (dispatch) => {

    // เรียกใช้ loadBarcode() ของ DB service
    let db = new DBService();
    let rows = await db.loadBarcode();
    
    dispatch({
        type: Types.BARCODE_DATA_LOADED,
        payload: rows
    })
}

// อย่าลืม Export ออกไปด้วย module อื่นจะได้ใช้ได้ 
export default {
    Types,
    barcodeScanned,
    requestBarcodeData
}
```

## 3. สั่งโหลดข้อมูล barcode มาแสดงหลังจากหน้า HomePage ถูกโหลด

เปิดไฟล์ `pages/home-page/HomePage.js`

เราจะสร้าง function ในส่วน `mapDispatchToProps` ก่อน

สังเกตว่า เป็นอีกที่ที่เราส่ง dispatch ให้ function เอาไปจัดการเอง อีกครั้ง

```js
const mapDispatchToProps = dispatch => {
    return {
        loadExistBarcodeData: () => actions.requestBarcodeData(dispatch)
    }
}
```

จากนั้นใน HomePage Component เราจะเรียกใช้ function ตอน component render เสร็จสมบูรณ์ 

```js
componentDidMount() {
        this.props.loadExistBarcodeData();
}
```