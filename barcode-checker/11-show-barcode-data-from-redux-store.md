
# 11. แสดงข้อมูล Barcode ในหน้า HomePage จาก Redux store

## 1. กำหนด Action และ state ใหม่ที่ได้จาก action ดังกล่าว ใน reducer 

เปิดไฟล์ `redux/reducers/app.reducer.js`

```js
import actions from "../actions"

const initialState = {

}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    // กำหนด case ในกรณีที่ barcode ถูกแสกนสำเร็จ
    case actions.Types.BARCODE_SCANNED: {
        // คืนค่า State เดิมทั้งหมด (อันเก่ามีกี่ property ก็จะ copy มาใส่ในอันใหม่นี้) 
        // พร้อมเพิ่ม/แทนที่ค่า scannedBarcode ด้วย payload ที่ได้จาก action
        return { ...state, scannedBarcode: payload }
    }

    default:
        return state
    }
}

```

## 2. ทำการเชื่อม HomePage component เข้า Redux store

เปิดไฟล์ `pages/home-page/HomePage.js`

เริ่มจาก import `connect` module มาจาก `redux`

```js
// ใช้ snippet ชื่อ redux ได้
import { connect } from 'react-redux'
```

จากนั้นเปลี่ยนการประกาศ `export default class HomePage` ให้เป็น `export class HomePage` อย่างเดียว

```js
// จาก
export default class HomePage extends Component 
// เป็น 
export class HomePage extends Component 
```

จากนั้นลงมาด้านล่างของ Class 

```js
// ใช้ snippet ชื่อ reduxmap ได้
const mapStateToProps = (state) => ({
    
})

const mapDispatchToProps = {
    
}

// เขียนกำหนด 2 ค่าด้านบนให้เชื่อมกับ redux store ผ่าน function connect() 
export default connect(mapStateToProps, mapDispatchToProps)(HomePage)
```

## 3. ใช้ค่า State Object ที่ได้จาก Redux Store

กำหนดค่า state object ที่ได้จาก redux store ให้กับ props ของ HomePage Component  

```js
 // จำส่วนนี้ที่อยู่ใน reducer ได้ไหม? 
    case actions.Types.BARCODE_SCANNED: {
        // object ตัวนี้แหละที่ส่งไปให้ Redux store
        // และ Redux store ส่ง object นี้ให้กับ Component ที่ connect กับตัว store 
        return { ...state, scannedBarcode: payload }
    }
```

ในที่นี้จะทำผ่าน function `mapStateToProps` ที่รับค่า parameter เป็น state มาจาก redux store

โดยค่าที่ return ออกจากฟังก์ชั่นนี้ จะถูกเอาไปใช้เป็นค่า `this.props` ของ HomePage Component

```js
const mapStateToProps = (state) => ({
    barcodeData: state.app.scannedBarcode
})

// จริงๆ เขียนแบบนี้ก็ได้ 
const mapStateToProps = (state) => {
    return {
        barcodeData: state.app.scannedBarcode
    };
}
```

## 4. นำค่า Props ที่ได้จาก `mapStateToProps` มาใช้ตามปกติ

ใน HomePage component ส่วน render() มีการเรียกใช้ค่า `barcodeData` จาก `this.props`

```js
    render() {

        const { barcodeData } = this.props;

        return (

            <Content>
                <Text>{barcodeData}</Text>
            </Content>

        )
    }
```

ทดสอบ Scan barcode ดู

## A. ไฟล์เต็ม `pages/home-page/HomePage.js`

```js
import React, { Component } from 'react'
import { View } from 'react-native'
import { Content, List, ListItem, Text, Body, Button, Icon } from 'native-base';
import PropTypes from 'prop-types'
import { connect } from 'react-redux'

export class HomePage extends Component {
    static navigationOptions = ({ navigation }) => {
        return {
            title: 'Home',
            headerRight: (
                <Button transparent
                    onPress={() => navigation.navigate('ScanPopup')}
                >
                    <Icon name='barcode' />
                </Button>
            )
        }
    };

    render() {

        const { barcodeData } = this.props;

        return (

            <Content>
                <Text>{barcodeData}</Text>
            </Content>

        )
    }
}

const mapStateToProps = (state) => ({
    barcodeData: state.app.scannedBarcode
})

const mapDispatchToProps = {

}

export default connect(mapStateToProps, mapDispatchToProps)(HomePage)
```
