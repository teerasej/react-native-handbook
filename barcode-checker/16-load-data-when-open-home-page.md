
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

}
```

## B. ไฟล์เต็ม `redux/actions.js`

```js
import DBService from "../services/db.service";

const Types = {
    BARCODE_SCANNED: 'BARCODE_SCANNED',
    BARCODE_DATA_LOADED: 'BARCODE_DATA_LOADED',
}

const barcodeScanned = async (dispatch,barcodeData) => {

    let db = new DBService();
    let rows = await db.saveBarcode(barcodeData);
    
    dispatch({
        type: Types.BARCODE_DATA_LOADED,
        payload: rows
    })
}

const requestBarcodeData = async (dispatch) => {
    let db = new DBService();
    let rows = await db.loadBarcode();
    
    dispatch({
        type: Types.BARCODE_DATA_LOADED,
        payload: rows
    })
}


export default {
    Types,
    barcodeScanned,
    requestBarcodeData,
}
```

## C. ไฟล์เต็ม `pages/home-page/HomePage.js`

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

    componentDidMount() {
        this.props.loadExistBarcodeData();
    }

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