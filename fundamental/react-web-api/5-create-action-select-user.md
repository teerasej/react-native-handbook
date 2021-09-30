
# 5. สร้าง action ที่แสดงถึงการเลือก user

เปิดไฟล์​ `redux/actions.js`

## 1. กำหนด Type ของ Action 

```js
export const actionTypes = {
    GET_USERS_SUCCESS: 'GET_USERS_SUCCESS',
    USER_SELECTED: 'USER_SELECTED'
}
```

## 2. สร้าง function action ที่จะสร้าง object ส่งเข้า Redux

```js
export const createAction_UserSelected = (user) => {
    return {
        type: actionTypes.USER_SELECTED,
        payload: user
    }
}
```

## A. ไฟล์เต็ม `redux/actions.js`

```js
export const actionTypes = {
    GET_USERS_SUCCESS: 'GET_USERS_SUCCESS',
    USER_SELECTED: 'USER_SELECTED'
}

export const createAction_UserSelected = (user) => {
    return {
        type: actionTypes.USER_SELECTED,
        payload: user
    }
}

export const startGetUser = async (dispatch) => {
    const url = 'https://randomuser.me/api/?results=50';


    const response = await fetch(url, {
        // กำหนด method GET
        method: 'GET',
        // กำหนด header เพื่อให้รับ และส่งข้อมูลแบบ JSON
        headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
        }
    });

    // ถ้า status 200 หมายถึง response ตอบกลับมาสมบูรณ์
    if (response && response.status === 200) {

        // แปลงข้อมูล JSON
        const json = await response.json();

        console.log('JSON:', json);
        console.log('Accounts:', json.results);

        // ส่งข้อมูล .results ที่ได้จาก Web API (เป็น Array) เข้า Redux
        dispatch({
            type: actionTypes.GET_USERS_SUCCESS,
            payload: json.results
        });

    } else {
        console.error(response.status, response);
    }

}

```