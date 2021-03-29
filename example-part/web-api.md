
# ดึงข้อมูลจาก Web API

- [Download Starter Project](https://www.dropbox.com/s/ds8gjpq7q1jheit/nextflow-contact-app-start-call-api.zip?dl=0)
- [Download Finish Project](https://www.dropbox.com/s/ujtx2gewv7phbxx/nextflow-contact-app-finish.zip?dl=0)

ในที่นี้ตัวโปรเจคจะมีส่วนที่ส่ง request ไปที่ Web API เพื่อนำข้อมูล User มาแสดงในแอพพลิเคชั่น และขั้นตอนทั้งหมดจะใช้ Redux ในการจัดการข้อมูลให้กับ Component ทั้งหมด



## 1. เขียน function สำหรับเรียกใช้ใน component

เราจะเริ่มจากการสร้าง function ที่ใช้ในการส่ง action object เข้า Redux store ด้วยวิธีนี้ จะทำให้เราสามารถส่ง action จาก component ไหนก็ได้ เพียงแค่ import function ของเราเข้าไปใช้งาน

เปิดไฟล์ `redux/action-make-call.js` 

```js
import { Types } from './actions'

export const startGetUser = async (dispatch) => {

    const url = 'https://randomuser.me/api/?results=50';

    
    const response = await fetch(url, {
        method: 'GET',
        headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
        }
    });

    if (response && response.status === 200) {
        const json = await response.json();

        console.log('JSON:',json);
        console.log('Accounts:', json.results);

        dispatch({
            type: Types.GET_USERS_SUCCESS,
            payload: json.results
        });

    } else {
        console.error(response.status, response);
    }
}
```

## 2. สร้าง props method โดยการกำหนดค่าใน `mapDispatchToProps`

หลังจากเราสร้าง function สำหรับส่ง action object แล้ว ในที่นี้เราจะนำมาใช้ใน HomePage Component 

เปิดไฟล์ `pages/home-page/HomePage.js` 
สร้าง method ชื่อ `startGetUser` ใน `mapDispatchToProps` 

```js
const mapDispatchToProps = (dispatch) => ({
    // สังเกตว่าในที่นี้ เราส่ง dispatch เข้าไปใช้ใน function โดยตรง
    startGetUser: () => actions.startGetUser(dispatch),
    selectUser: (user) => dispatch(actions.selectUser(user))
})
```

## 3. เริ่มเรียกข้อมูลตอนหน้า HomePage ถูกโหลดขึ้นมาใช้งาน

โดยการเรียกใช้  `this.props.startGetUser()` ใน method `componentDidMount()`

```js
componentDidMount() {
    this.props.startGetUser();
}
```

## การใช้งาน Axios ใน Redux

[ดูเพิ่มเติม](web-api-axios.md)



## A. ไฟล์เต็ม `pages/home-page/HomePage.js`

```jsx
import React, { Component } from 'react'
import { View } from 'react-native'
import { connect } from 'react-redux'
import { Content, List, ListItem, Text, Body, Button, Icon, Left, Right, Thumbnail } from 'native-base';
import actions from "../../redux/actions";

export class HomePage extends Component {

    static navigationOptions = {
        title: 'Contacts'
    };

    componentDidMount() {
        this.props.startGetUser();
    }

    openDetail = (user) => {
        this.props.selectUser(user);
        this.props.navigation.navigate('Detail');
    }

    render() {

        if(this.props.users == undefined){
            return <Text>Loading</Text>
        }

        return (
            <Content>
                <List>
                    {
                        this.props.users.map((item, index) => {
                            return (
                                <ListItem thumbnail
                                    button
                                    key={index}
                                    onPress={() => {this.openDetail(item)}}
                                >
                                    <Left>
                                        <Thumbnail source={{ uri: item.picture.thumbnail }} />
                                    </Left>
                                    <Body>
                                        <Text>{item.name.first} {item.name.last}</Text>
                                        <Text note>{item.phone}</Text>
                                    </Body>
                                    <Right></Right>
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
    users: state.app.users
})

const mapDispatchToProps = dispatch => ({
    startGetUser: () => actions.startGetUser(dispatch),
    selectUser: (user) => dispatch(actions.selectUser(user))
})

export default connect(mapStateToProps, mapDispatchToProps)(HomePage)

```

## B. ไฟล์เต็ม `redux/actions.js`

```js

import call from 'react-native-phone-call'

const Types = {
    GET_USERS_SUCCESS: 'GET_USERS_SUCCESS',
    USER_SELECTED: 'USER_SELECTED'
}

const startGetUser = async (dispatch) => {

    const url = 'https://randomuser.me/api/?results=50';

    const response = await fetch(url, {
        method: 'GET',
        headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
        }
    });

    if (response && response.status === 200) {
        const json = await response.json();

        console.log('JSON:',json);
        console.log('Accounts:', json.results);

        dispatch({
            type: Types.GET_USERS_SUCCESS,
            payload: json.results
        });

    } else {
        console.error(response.status, response);
    }
}

const selectUser = (user) => {
    return {
        type: Types.USER_SELECTED,
        payload: user
    }
}

const makeCall = (phoneNumber) => {
    call({number: phoneNumber}).catch(console.error);
}

export default {
    Types,
    startGetUser,
    selectUser,
    makeCall
}
```
