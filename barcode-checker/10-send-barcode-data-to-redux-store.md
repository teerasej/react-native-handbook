
# 10. ส่งข้อมูล Barcode ได้เข้า Redux store

## 1. สร้าง Action ที่ใช้ในระบบ

สร้างไฟล์ 

```js

// กำหนดชื่อประเภทของ action (เหตุการณ์ต่างๆ) ที่เกิดในระบบและมีผลต่อข้อมูลโดยรวม
const Types = {
    // ใช้ในกรณีที่มีการแสกนบาร์โค้ดเสร็จแล้ว
    BARCODE_SCANNED: 'BARCODE_SCANNED'
}

// function สำหรับสร้าง Action object ที่จะส่งเข้า redux store
const barcodeScanned = (barcodeData) => {
    return {
        type: Types.BARCODE_SCANNED,
        payload: barcodeData
    };
}

// export เอาไปใช้งาน
export default {
    Types,
    barcodeScanned
}
```

## 2. ทำการเชื่อง ScanPage component เข้า Redux store

เปิดไฟล์ `pages/scan-page/ScanPage.js`

เริ่มจาก import `connect` module มาจาก `redux`

```js
// ใช้ snippet ชื่อ redux ได้
import { connect } from 'react-redux'
```

จากนั้นเปลี่ยนการประกาศ `export default class ScanPage` ให้เป็น `export class ScanPage` อย่างเดียว

```js
// จาก
export default class ScanPage extends Component 
// เป็น 
export class ScanPage extends Component 
```

จากนั้นลงมาด้านล่างของ Class 

```js
// ใช้ snippet ชื่อ reduxmap ได้
const mapStateToProps = (state) => ({
    
})

const mapDispatchToProps = {
    
}

// เขียนกำหนด 2 ค่าด้านบนให้เชื่อมกับ redux store ผ่าน function connect() 
export default connect(mapStateToProps, mapDispatchToProps)(ScanPage)
```

## 3. สร้าง Action ที่มีข้อมูล barcode ส่งเข้า Redux store

กำหนด dispatch function ให้กับ `props` ของ ScanPage

```js
const mapDispatchToProps = dispatch => {
    return {
        barcodeScanned: (barcodeData) => dispatch(actions.barcodeScanned(barcodeData))
    }
}
```

จากนั้น เรียกใช้ในส่วนที่เราได้ข้อมูลจาก Barcode Scanner มาแล้ว

```js
handleBarCodeScanned = ({ type, data }) => {
        console.log(`Bar code with type ${type} and data ${data} has been scanned!`);
        
        // ส่งข้อมูลเป็น action ผ่าน dispatch function เพื่อส่งเข้า redux store
        this.props.barcodeScanned(data);

        // ปิดหน้า Scan
        this.closePopUp();
    };
```



