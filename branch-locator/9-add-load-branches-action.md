
# 9. สร้าง Action โหลดข้อมูลสาขา

## 1. สร้าง action function ที่จะทำให้โหลดข้อมูลสาขาได้

เปิดไฟล์ `redux/actions.js` และเขียน function `getBranchLocation` ให้สมบูรณ์

```js

const getBranchLocation = async (dispatch) => {
    const response = await fetch('http://localhost:3000/api/branches', {
        method: 'GET',
        headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json',
            // แทรก token เข้าไปใน header เพื่อเข้าถึงข้อมูล
            'Authorization': `Bearer ${token}`
        }
    })

    console.log('Response:', response);

    if (response && response.status === 200) {
        const json = await response.json();

        // ถ้าได้ข้อมูลสาขามา ให้ส่งไปเก็บใน app state 
        console.log('Branches:', json.branches);
        dispatch({
            type: Types.GET_BRANCH_LIST_SUCCESS,
            payload: json.branches
        })

    } else {
        console.error(response.status, response.message);
    }
}
```

## 2. เพิ่มข้อมูลสาขาหลังจากได้รับจาก API

เปิดไฟล์​ `redux/reducers/app.reducer.js`

```js
case actions.Types.GET_BRANCH_LIST_SUCCESS: {
        return { ...state, branches: payload }
}
```

## A. ไฟล์เต็ม `redux/actions.js`

```js
import locationService from "../services/location.service"
// Android Emulator's default localhost ip: 10.0.2.2
let token = '';


const Types = {
    LOCATION_DEVICE_FOUND: 'LOCATION_DEVICE_FOUND',
    SIGN_IN_SUCCESS: 'SIGN_IN_SUCCESS',
    GET_BRANCH_LIST_SUCCESS: 'GET_BRANCH_LIST_SUCCESS'
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

const getBranchLocation = async (dispatch) => {
    const response = await fetch('http://localhost:3000/api/branches', {
        method: 'GET',
        headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${token}`
        }
    })

    console.log('Response:', response);

    if (response && response.status === 200) {
        const json = await response.json();
        console.log('Branches:', json.branches);
        dispatch({
            type: Types.GET_BRANCH_LIST_SUCCESS,
            payload: json.branches
        })

    } else {
        console.error(response.status, response.message);
    }
}

export default {
    Types,
    getCurrentLocation,
    startSignIn,
    getBranchLocation
}
```

## B. ไฟล์เต็ม `redux/reducers/app.reducer.js`

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

    case actions.Types.GET_BRANCH_LIST_SUCCESS: {
        return { ...state, branches: payload }
    }

    default:
        return state
    }
}
```