
# 13. ควบคุม Navigation จาก Saga

## 1. ปิดปุ่ม back ใน HomePage 

```js
// pages/home-page/HomePage.js

static navigationOptions = ({ navigation }) => {
        return {
          headerTitle: <Text>Home</Text>,
          headerRight: (
            <Button transparent onPress={() => {
                console.log('ok')
                navigation.navigate('CreateNote')
            }}>
              <Icon name='add'/>
            </Button>
          ),
          // ปิดปุ่มทางด้านซ้าย
          headerLeft: null
        };
      };
```

## 2. สร้าง Service class สำหรับเรียกใช้ nav ที่ไหนก็ได้

```js
// redux/NavigationActionClass.js

class NavigationActionsClass {

    setNavigator(navigator) {
      this.navigator = navigator
    }

    push = (params) => this.navigator && this.navigator.push(params)
    pop = (params) => this.navigator && this.navigator.pop(params)
    resetTo = (params) => this.navigator && this.navigator.resetTo(params)
    toggleDrawer = (params) => this.navigator && this.navigator.toggleDrawer(params)
    navigate = (routerName) => {
        console.log('Router Name', routerName);
        this.navigator && this.navigator.navigate(routerName)
    }
}

export let NavigationActions = new NavigationActionsClass() 
```

## 3. ส่งผ่าน Navigation ให้ service class

เราจำเป็นต้องส่ง `props.navigation` ให้กับ `NavigationActionsClass` ดังนั้นหน้าแรกที่โหลดจะเป็นตัวรับหน้าที่นี้ 

```js
// pages/login-page/LoginPage.js

import { NavigationActions } from '../../redux/NavigationActionClass';

export class LoginPage extends Component {

    constructor(props){
        super(props);

        NavigationActions.setNavigator(this.props.navigation);
    }

}
```

## 4. เรียกใช้ navigation service เพื่อเปิดไปหน้าที่ต้องการ

```js
// redux/sagas/signin.saga.js

import { NavigationActions } from "../NavigationActionClass";


export function* doSignIn(action) {
    //..

    if (response && response.status === 200) {
            const json = yield call([response, response.json]);
            yield put({ type: actions.ActionTypes.SIGN_IN_SUCCESS, payload: json });

            // เรียกใช้ method navigate ผ่าน call effect
            yield call(NavigationActions.navigate, 'Home')
        } else {
            yield put({ type: actions.ActionTypes.SIGN_IN_FAILED, payload: result });
        }
}
```

## 4.A ไฟล์เต็ม `// redux/sagas/signin.saga.js`

```js
import { takeEvery, put, call } from "redux-saga/effects";
import actions from "../actions";
import { NavigationActions } from "../NavigationActionClass";


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
            yield call(NavigationActions.navigate, 'Home')
        } else {
            yield put({ type: actions.ActionTypes.SIGN_IN_FAILED, payload: result });
        }

    } catch (error) {
        yield put({ type: actions.ActionTypes.SIGN_IN_FAILED, payload: error });
    }




}

export default signInWatch;

```

## ทดสอบ Saga กับ Web API

1. [ดาวน์โหลดโปรเจค node web api project จากที่นี่](https://www.dropbox.com/s/bglkcul22zu4rnv/web-api-branch-service.zip?dl=0)
2. แตก zip 
3. เปิด Terminal ขึ้นมาในโฟลเดอร์โปรเจค
4. รันคำสั่ง `npm i` เพื่อติดตั้ง node module
5. รันคำสั่ง `node index` เพื่อรัน web server
6. ทดสอบใช้งาน login ด้วยข้อมูลต่อไปนี้

- username: a
- password: pass
