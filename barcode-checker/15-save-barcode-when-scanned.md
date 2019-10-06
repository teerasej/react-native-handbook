
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
const mapDispatchToProps = (dispatch) => {
    return {
        barcodeScanned: (barcodeData) => dispatch(actions.barcodeScanned(barcodeData))
    }
}
```

ไปเป็นแบบนี้ 

```js
const mapDispatchToProps = dispatch => {
    return {
        barcodeScanned: (barcodeData) => actions.barcodeScanned(dispatch, barcodeData) // เห็นที่เราส่ง dispatch เข้าไปใน action function ไหม?
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

## 5. ปรับหน้า HomePage ให้แสดงข้อมูลจาก `barcodes` ของ app state  

เปิดไฟล์ `pages/home-page/HomePage.js`

```js
// ดึงข้อมูล Barcodes ออกมาจาก props
// ถ้าไม่มีข้อมูล ให้แสดงข้อความ
// ถ้ามี ให้เอา array มาวนแสดงใน list
render() {

        const { barcodes } = this.props;

        // console.log(barcodes);

        if (!barcodes) {
            return (
                <View style={{
                    flex: 1,
                    justifyContent: 'center',
                    alignItems: 'center'
                }}>
                    <Text>Scan something...</Text>
                </View>
            );
        }

        return (

            <Content>
                <List>
                    {
                        barcodes.map((item, index) => {
                            return (
                                <ListItem key={index} button={true}>
                                    <Text>{item.barcodeData}</Text>
                                </ListItem>
                            )
                        })
                    }
                </List>
            </Content>

        )
}

// ส่วนนี้ดึงข้อมูลมาจาก app state
const mapStateToProps = (state) => ({
    barcodeData: state.app.scannedBarcode,
    barcodes: state.app.barcodes  
})
```

## A. ไฟล์เต็ม `pages/home-page/HomePage.js`

```js
import React, { Component } from 'react'
import { View } from 'react-native'
import { Content, List, ListItem, Text, Body, Button, Icon } from 'native-base';
import PropTypes from 'prop-types'
import { connect } from 'react-redux'
import actions from '../../redux/actions';

export class HomePage extends Component {
    static navigationOptions = ({ navigation }) => {
        return {
            headerTitle: <Text>Home</Text>,
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

        const { barcodes } = this.props;

        // console.log(barcodes);

        if (!barcodes) {
            return (
                <View style={{
                    flex: 1,
                    justifyContent: 'center',
                    alignItems: 'center'
                }}>
                    <Text>Scan something...</Text>
                </View>
            );
        }

        return (

            <Content>
                <List>
                    {
                        barcodes.map((item, index) => {
                            return (
                                <ListItem key={index} button={true}>
                                    <Text>{item.barcodeData}</Text>
                                </ListItem>
                            )
                        })
                    }
                </List>
            </Content>

        )
    }
}

const mapStateToProps = (state) => ({
    barcodeData: state.app.scannedBarcode,
    barcodes: state.app.barcodes  
})

const mapDispatchToProps = dispatch => {
    return {
        loadExistBarcodeData: () => actions.requestBarcodeData(dispatch)
    }
}

export default connect(mapStateToProps, mapDispatchToProps)(HomePage)

```

## B. ไฟล์เต็ม `services/db.service.js`

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


}
```

## C. ไฟล์เต็ม `redux/actions.js`

```js
import DBService from "../services/db.service";

const Types = {
    BARCODE_SCANNED: 'BARCODE_SCANNED',
    BARCODE_DATA_LOADED: 'BARCODE_DATA_LOADED'
}

const barcodeScanned = async (dispatch,barcodeData) => {

    let db = new DBService();
    let rows = await db.saveBarcode(barcodeData);
    
    dispatch({
        type: Types.BARCODE_DATA_LOADED,
        payload: rows
    })
}

const barcodeSelected = (dispatch, barcodeData) => {
    dispatch({
        type: Types.BARCODE_LIST_ITEM_SELECTED,
        payload: barcodeData
    })
}


export default {
    Types,
    barcodeScanned,
    barcodeSelected,
}
```

## D. ไฟล์เต็ม `pages/scan-page/ScanPage.js`

```js
import React, { Component } from 'react'
import { View, StyleSheet } from 'react-native'
import { connect } from 'react-redux'
import { Container, Header, Title, Content, Right, Left, Button, Icon, Text, Body } from 'native-base';

import { BarCodeScanner } from 'expo-barcode-scanner';
import * as Permissions from 'expo-permissions';
import actions from '../../redux/actions';

export class ScanPage extends Component {

    state = {
        hasCameraPermission: null
    };

    closePopUp = () => {
        this.props.navigation.goBack();
    }

    async componentDidMount() {
        this.getPermissionsAsync();
    }

    getPermissionsAsync = async () => {
        const { status } = await Permissions.askAsync(Permissions.CAMERA);
        this.setState({ hasCameraPermission: status === 'granted' });
    };

    handleBarCodeScanned = ({ type, data }) => {
        console.log(`Bar code with type ${type} and data ${data} has been scanned!`);
        this.props.barcodeScanned(data);
        this.closePopUp();
    };

    render() {

        const { hasCameraPermission, scanned } = this.state;

        if (hasCameraPermission === false) {
            return <Text>No access to camera</Text>;
        }

        return (

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

                <View
                    style={{
                        flex: 1
                    }}>
                    <BarCodeScanner
                        onBarCodeScanned={this.handleBarCodeScanned}
                        style={StyleSheet.absoluteFillObject}
                    />
                </View>
            </Container>
        )
    }
}

const mapStateToProps = (state) => ({

})

const mapDispatchToProps = dispatch => {
    return {
        barcodeScanned: (barcodeData) => dispatch(actions.barcodeScanned(dispatch, barcodeData))
    }
}

export default connect(mapStateToProps, mapDispatchToProps)(ScanPage)
```