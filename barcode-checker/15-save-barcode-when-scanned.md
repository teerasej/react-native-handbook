
# 15. บันทึกข้อมูลเมื่อมีการแสกน Barcode

## 1. สร้าง function ที่บันทึก barcode ลง database 

เปิดไฟล์ `services/db.service.js`

```js
saveBarcode(barcodeData: String) {

        // return เป็น Promise object อีกครั้ง 
        return new Promise((resolve, reject) => {

            // เราสามารถสั่งรัน transaction ได้ 
            db.transaction(
                tx => {
                    
                    // ใส่ข้อมูลลงไปใน database
                    tx.executeSql(`INSERT INTO ${tableName} (barcodeData) values (?)`, [barcodeData]);

                    // Query ข้อมูลจาก Database 
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

## 2. ปรับ Action function ให้เรียกใช้ DB service และ dispatch event ได้ 

เปิดไฟล์ `redux/actions.js`

แต่เดิมเราจะมี function `barcodeScanned` ที่ใช้สร้าง action object 

```js
const barcodeScanned = (barcodeData) => {
    return {
        type: Types.BARCODE_SCANNED,
        payload: barcodeData
    };
}
```

แต่เราจะเปลี่ยนใหม่ ให้ function `barcodeScanned` สามารถ dispatch action เองได้ 

โดยจะมีการบันทึกข้อมูล barcode ลง database 

ก่อนที่จะได้ข้อมูล barcode ใน database ใส่ใน action ก่อน dispatch เข้า Redux store 

```js
const barcodeScanned = async (dispatch,barcodeData) => {

    let db = new DBService();
    let rows = await db.saveBarcode(barcodeData);
    
    dispatch({
        type: Types.BARCODE_DATA_LOADED,
        payload: rows
    })
}
```

## 3. แก้ไขให้ Scan Page ส่ง dispatch ให้ action function

เปิดไฟล์ `pages/scan-page/ScanPage.js`

เราจะเปลี่ยนจาก dispatch action โดยตรง แบบนี้

```js
const mapDispatchToProps = dispatch => {
    return {
        barcodeScanned: (barcodeData) => dispatch(actions.barcodeScanned(barcodeData))
    }
}
```

ไปเป็นแบบนี้ 

```js
const mapDispatchToProps = dispatch => {
    return {
        barcodeScanned: (barcodeData) => actions.barcodeScanned(dispatch, barcodeData)
    }
}
```

## 4. ปรับให้ app reducer รับ action 

เปิดไฟล์ `redux/reducers/app.reducer.js`

```js
import actions from "../actions"

const initialState = {
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case actions.Types.BARCODE_SCANNED: { 
        return { ...state, barcodeScanned: payload }
    }

    // เพิ่มการรับ action BARCODE_DATA_LOADED และนำ payload ที่ได้ ใส่เข้าไปใน state ในชื่อของ barcodes
    case actions.Types.BARCODE_DATA_LOADED: { 
        // console.log('barcodes arrived', payload);
        return { ...state, barcodes: [...payload] }
    }

    default:
        return state
    }
}

```