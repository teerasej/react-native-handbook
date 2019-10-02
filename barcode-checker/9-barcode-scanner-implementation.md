
# 9. สร้างระบบ Scan Barcode

> [Expo Barcode Scanner](https://docs.expo.io/versions/latest/sdk/bar-code-scanner/)

## 1. ติดตั้ง Plugin 

รันคำสั่งใน Terminal

```bash
expo install expo-barcode-scanner
```

## 2. import package ที่เกี่ยวข้อง

เปิดไฟล์ `pages/scan-page/ScanPage.js`

```js
// Component ที่แสดงภาพจากกล้อง และเป็นตัวอ่านค่า Barcode
import { BarCodeScanner } from 'expo-barcode-scanner';
// ตัวขอ Permission ในการเข้าถึง Camera
import * as Permissions from 'expo-permissions';
```

## 3. กำหนดค่า State สำหรับใช้ในการอัพเดต UI

- **hasCameraPermission** ไว้เช็คว่าผู้ใช้อนุญาตหรือยัง

```js
export class ScanPage extends Component {

    state = {
        hasCameraPermission: null
    };
```

## 4. เขียนคำสั่งขอ Permission

เปิดไฟล์ `pages/scan-page/ScanPage.js`

```js
export class ScanPage extends Component {

    // ใช้ Life cycle method `componentDidMount`
    // เพื่อรันโค้ดหลังจาก ScanPage render ครั้งแรกเสร็จแล้ว
    async componentDidMount() {
        this.getPermissionsAsync();
    }


    getPermissionsAsync = async () => {
        // ขอ permission ใช้งานกล้อง
        const { status } = await Permissions.askAsync(Permissions.CAMERA);
        // set State ใหม่โดยกำหนดค่า hasCameraPermission เพื่อใช้งานใน method render()
        this.setState({ hasCameraPermission: status === 'granted' });
    };
```

## 5. แสดงข้อความเตือน ในกรณีที่ผู้ใช้ไม่ได้กดอนุญาตให้แอพเข้าถึงกล้อง

เปิดไฟล์ `pages/scan-page/ScanPage.js`

ดึงค่า `hasCameraPermission` ออกมาจาก state เพื่อเลือกแสดง UI ตามสถานการณ์ ในที่นี้มีกรณีที่ผู้ใช้กดอนุญาต และไม่ได้กดอนุญาต

```js
render() {

        const { hasCameraPermission } = this.state;

        // ถ้าค่า hasCameraPermission เป็น false 
        // เราจะแสดง UI เป็นข้อความแทน
        if (hasCameraPermission === false) {
            return <Text>No access to camera</Text>;
        }

        return (
```

## 6. แสดงภาพจากกล้อง ด้วย Barcode Scanner

เปิดไฟล์ `pages/scan-page/ScanPage.js`

```js
// import StyleSheet component มาใช้งานจาก React-native
// StyleSheet มีค่าตายตัวที่เราเอามาใช้ในกรณีนี้ได้
import { View, StyleSheet } from 'react-native'

//..

render() {
    return  (

            <Container>
                <Header>
                    <Left>
                    </Left>
                    <Body>
                        <Title>Scanner</Title>
                    </Body>
                    <Right>
                        <Button transparent onPress={this.closePopUp}>
                            <Icon name='close' />
                        </Button>
                    </Right>
                </Header>
                {/*
                 ลบส่วน Content component ของ Nativebase ออกไป 
                 เพื่อเอา View component ของ react-native มาใช้แทน

                 - ค่า flex: 1 หมายถึงขยายให้เต็มพื้นที่
                */}
                <View
                    style={{
                        flex: 1
                    }}>
                    {/* แสดงตัวอ่านบาร์โค้ด */}
                    <BarCodeScanner
                        style={StyleSheet.absoluteFillObject}
                    />
                </View>
            </Container>
}
```

## 7. สร้าง function รับ event หลังจาก scan ได้

```js
handleBarCodeScanned = ({ type, data }) => {
    alert(`Bar code with type ${type} and data ${data} has been scanned!`);
};
```

และนำไปใช้ใน Barcode Scanner Component 

```jsx
<BarCodeScanner
        onBarCodeScanned={this.handleBarCodeScanned}
        style={StyleSheet.absoluteFillObject}
    />
```