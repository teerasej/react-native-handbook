
# 11. แสดงข้อมูล Barcode ในหน้า HomePage จาก Redux store

## 1. ใช้ navigation ย้อนกลับไปที่ screen ก่อนหน้า หลัง scan barcode สำเร็จ

เปิดไฟล์ `pages/scan-page/ScanPage.js`

```js
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
