
# 8. สร้าง Sign In Action


## 1. สร้าง action function

เปิดไฟล์ `redux/actions.js` 

เขียน function `startSignIn` ให้สมบูรณ์

```js
// Android Emulator's default localhost ip: 10.0.2.2
// แชร์ token กับ action อื่นๆ 
let token = '';

const startSignIn = async (dispatch, navigation, username, password) => {

    // สร้าง object สำหรับแปลงเป็น JSON เพื่อส่งให้ Web API
    const signInInfo = {
        username: username,
        password: password
    };

    const response = await fetch('http://localhost:3000/api/auth/signin', {
        // เลือกส่งแบบ HTTP Post
        method: 'POST',
        // ตั้งค่า header ให้เป็น JSON
        headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
        },
        // แทรกข้อมูล JSON ไปในส่วนของ body
        body: JSON.stringify(signInInfo)
    })

    // แสดง response ที่ส่งกลับมา
    console.log('Response:', response);

    if (response && response.status === 200) {

        // ถ้า response เป็น 200 จะมี token กลับมา เราจะส่ง token ไปกับ action เพื่อเก็บไว้ใน app state
        const json = await response.json();
        console.log('Token:', json.token);

        token = json.token;

        dispatch({
            type: Types.SIGN_IN_SUCCESS,
            payload: json.token
        })

        console.log('going to home page')
        navigation.navigate('Home');

    } else {
        console.error(response.status, response.message);
    }
}

```

## 2. นำ token ที่ได้ ใส่ใน app state

เพื่อให้ component อื่นๆ เข้าถึงได้

เปิดไฟล์ `redux/reducers/app.reducer.js`

```js
case actions.Types.SIGN_IN_SUCCESS: {
        return { ...state, token: payload }
    }
```

## 3. เรียกใช้ action ในหน้า Login Page 

เปิดไฟล์ `pages/login-page/LoginPage.js`

```js
import actions from '../../redux/actions';


export class LoginPage extends Component {

    //...

    onSignIn = (values) => {
        console.log(values);
        // นอกจากส่ง username, password แล้ว
        // ส่ง navigation เพื่อให้สามารถเปิดไปยังหน้า home page ได้
        this.props.startSignIn(this.props.navigation, values.username, values.password);
    }
    
    //...
}

//..

const mapDispatchToProps = dispatch => {
    return {
        startSignIn: (username,password) => actions.startSignIn(dispatch, navigation, username, password)
    }
}


```

## A. ไฟล์เต็ม `redux/reducers/app.reducer.js`

```js
import actions from "../actions"

const initialState = {
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case actions.Types.LOCATION_DEVICE_FOUND: {
        return { ...state, currentLocation: payload }
    }

    case actions.Types.SIGN_IN_SUCCESS: {
        return { ...state, token: payload }
    }

    default:
        return state
    }
}
```

## B. ไฟล์เต็ม `redux/actions.js` 

```js
import locationService from "../services/location.service"
// Android Emulator's default localhost ip: 10.0.2.2
// แชร์ token กับ action อื่นๆ 
let token = '';


const Types = {
    LOCATION_DEVICE_FOUND: 'LOCATION_DEVICE_FOUND',
    SIGN_IN_SUCCESS: 'SIGN_IN_SUCCESS'
}

const getCurrentLocation = async (dispatch) => {

    const {location, errorMessage } = await locationService();

    if(location){
        dispatch({
            type: Types.LOCATION_DEVICE_FOUND,
            payload: location
        })
    } else if (errorMessage) {
        console.error(errorMessage);
    }

}

const startSignIn = async (dispatch, navigation, username, password) => {

    const signInInfo = {
        username: username,
        password: password
    };

    const response = await fetch('http://localhost:3000/api/auth/signin', {
        method: 'POST',
        headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
        },
        body: JSON.stringify(signInInfo)
    })

    console.log('Response:', response);

    if (response && response.status === 200) {
        const json = await response.json();
        console.log('Token:', json.token);

        token = json.token;

        dispatch({
            type: Types.SIGN_IN_SUCCESS,
            payload: json.token
        })

        console.log('going to home page')
        navigation.navigate('Home');

    } else {
        console.error(response.status, response.message);
    }
}

export default {
    Types,
    getCurrentLocation,
    startSignIn
}
```

## C. ไฟล์เต็ม LoginPage.js

```js
import React, { Component } from 'react'
import { View, Text } from 'react-native'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'
import LoginForm from './LoginForm';
import { Content } from 'native-base';
import actions from '../../redux/actions';


export class LoginPage extends Component {

    static navigationOptions = {
        title: 'Login'
    }

    onSignIn = (values) => {
        console.log(values);
        this.props.startSignIn(this.props.navigation, values.username, values.password);
    }

    render() {
        return (
            <Content padder>
                <LoginForm onSubmit={this.onSignIn} />
            </Content>
        )
    }
}

const mapStateToProps = (state) => ({

})

const mapDispatchToProps = dispatch => {
    return {
        startSignIn: (navigation, username,password) => actions.startSignIn(dispatch, navigation, username, password)
    }
}

export default connect(mapStateToProps, mapDispatchToProps)(LoginPage)
```