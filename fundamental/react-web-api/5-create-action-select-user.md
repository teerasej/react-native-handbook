
# 5. สร้าง action ที่แสดงถึงการเลือก user

เปิดไฟล์​ `redux/actions.js`

## 1. กำหนด Type ของ Action 

```js
const Types = {
    GET_USERS_SUCCESS: 'GET_USERS_SUCCESS',
    USER_SELECTED: 'USER_SELECTED'
}
```

## 2. สร้าง function action ที่จะสร้าง object ส่งเข้า Redux

```js
const selectUser = (user) => {
    return {
        type: Types.USER_SELECTED,
        payload: user
    }
}

// อย่าลืม export ฟังก์ชั่น selectUser ออกไปด้วย ไม่งั้นไฟล์อื่นจะเรียกใช้ไม่ได้
export default {
    Types,
    startGetUser,
    selectUser
} 
```

## A. ไฟล์เต็ม `redux/actions.js`

```js
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

export default {
    Types,
    startGetUser,
    selectUser
}
© 2019 GitHub, Inc.
Te
```