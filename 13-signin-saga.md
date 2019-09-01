# 12. สร้าง และใช้งาน Saga

- [Redux saga](https://github.com/redux-saga/redux-saga)

## 1. สร้าง action เพื่อเริ่ม flow ของการ login

```js
// redux/actions.js

const ActionTypes = {
    SAVE_NEW_NOTE: 'SAVE_NEW_NOTE',
    SIGN_IN_START: "SIGN_IN_START",
    SIGN_IN_SUCCESS: "SIGN_IN_SUCCESS",
    SIGN_IN_FAILED: "SIGN_IN_FAILED",
}

const saveNewNote = (message) => (
    {
        type: ActionTypes.SAVE_NEW_NOTE,
        payload: message
    }
)

const startSignIn = (username, password) => (
    {
        type: ActionTypes.SIGN_IN_START,
        payload: { username: username, password: password}
    }
)

export default {
    ActionTypes,
    saveNewNote,
    startSignIn
}

```

## 2. แก้ไข Login Page ให้ส่ง Action ได้ 

```jsx
import React, { Component } from 'react'
import { View, Text } from 'react-native'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'
import LoginForm from './LoginForm';
import { Content } from 'native-base';
import actions from '../../redux/actions';

export class LoginPage extends Component {

    static navigationOptions = ({ navigation }) => {
        return {
            headerTitle: <Text>Login</Text>
        };
    };

    onSignIn = (values) => {
        console.log(values);
        // dispatch action ที่เตรียมไว้
        this.props.signIn(values.username, values.password);
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

// โยง function props เข้ากับ dispatch
const mapDispatchToProps = dispatch => {
    return {
        signIn: (username, password) => dispatch(actions.startSignIn(username,password))
    }
}

export default connect(mapStateToProps, mapDispatchToProps)(LoginPage)

```

## 3. สร้าง root saga

```js
// redux/sagas/root.saga.js

import { all } from 'redux-saga/effects'

export default function* rootSaga() {
    yield all([

    ])
} 

```

## 4. แก้ไขไฟล์​ store.js ให้ใช้งาน saga ได้ 

```js
// redux/store.js

import { createStore, applyMiddleware, compose } from 'redux';
import { logger } from 'redux-logger';

import createRootReducer from "./reducers/root.reducer";

// เรียกใช้งาน createSagaMiddleware
import createSagaMiddleware from 'redux-saga'
// เรียกใช้งาน rootSaga
import rootSaga from './sagas/root.saga';
// สร้าง sagaMiddleware
const sagaMiddleware = createSagaMiddleware()

export default function configureStore() {
    const store = createStore(
        createRootReducer(),
        compose(
            applyMiddleware(
                logger,
                // เพิ่ม sagaMiddleware เข้าไปใน store
                sagaMiddleware
            ),
        ),
    );

    // โหลด rootSaga เข้าไปทำงาน
    sagaMiddleware.run(rootSaga);

    return store;
}
```

## 5. สร้าง Saga สำหรับใข้ในการ signin

```js
// redux/sagas/signin.saga.js

import { takeEvery, put, call } from "redux-saga/effects";
import actions from "../actions";
import "isomorphic-fetch";

function* signInWatch() {
    console.log('catching')
    yield takeEvery(actions.ActionTypes.SIGN_IN_START, doSignIn)
}

export function* doSignIn(action) {

    const authInfo = action.payload;

    try {
        const response = yield call(fetch, 'http://localhost:3002/api/auth/signin', {
            method: 'POST',
            headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(authInfo)
        })

        console.log(response);

        if (response && response.status === 200) {
            const json = yield call([response, response.json]);
            yield put({ type: actions.ActionTypes.SIGN_IN_SUCCESS, payload: json });

        } else {
            yield put({ type: actions.ActionTypes.SIGN_IN_FAILED, payload: result });
        }

    } catch (error) {
        yield put({ type: actions.ActionTypes.SIGN_IN_FAILED, payload: error });
    }

}

export default signInWatch; 
```

## 6. นำ signIn saga มาเพิ่มใน Root Saga 

```js
// redux/sagas/root.saga.js

import { all } from 'redux-saga/effects'
import signIn from "./signin.saga";

export default function* rootSaga() {
    yield all([
        signIn()
    ])
} 
```

## 7. สร้าง app.reducer.js 

สร้างไฟล์ `redux/reducers/app.reducer.js`

```js
import actions from "../actions";

const initialState = {
    token: ''
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case actions.ActionTypes.SIGN_IN_SUCCESS:
        return { ...state, token: payload.token }

    default:
        return state
    }
}

```

## 8. ทดสอบ Saga กับ Web API

1. [ดาวน์โหลดโปรเจค node web api project จากที่นี่](https://www.dropbox.com/s/bglkcul22zu4rnv/web-api-branch-service.zip?dl=0)
2. แตก zip 
3. เปิด Terminal ขึ้นมาในโฟลเดอร์โปรเจค
4. รันคำสั่ง `npm i` เพื่อติดตั้ง node module
5. รันคำสั่ง `node index` เพื่อรัน web server
6. ทดสอบใช้งาน login ด้วยข้อมูลต่อไปนี้

- username: a
- password: pass
