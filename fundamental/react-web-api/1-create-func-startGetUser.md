

# 1. สร้าง function startGetUser

เพื่อส่ง request เรียกข้อมูลไปที่ Web API และส่งข้อมูลไปกับ action ชื่อ `GET_USERS_SUCCESS` เข้า reducer

## 1. กำหนด Action Types

```js
const Types = {
    GET_USERS_SUCCESS: 'GET_USERS_SUCCESS'
}
```

## 2. ใช้ fetch function ในการส่ง request กับ web api

```js
const startGetUser = async (dispatch) => {

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

}
```

## 3. ตรวจสอบ response และส่งข้อมูลเข้า dispatch 

```js
// ถ้า status 200 หมายถึง response ตอบกลับมาสมบูรณ์
if (response && response.status === 200) {

    // แปลงข้อมูล JSON
    const json = await response.json();

    console.log('JSON:',json);
    console.log('Accounts:', json.results);

    // ส่งข้อมูล .results ที่ได้จาก Web API (เป็น Array) เข้า Redux
    dispatch({
        type: Types.GET_USERS_SUCCESS,
        payload: json.results
    });

} else {
    console.error(response.status, response);
}
```

## 4. สุดท้ายส่ง function ออกไปทาง export default 

```js
export default {
    Types,
    startGetUser
} 
```

## A. ไฟล์เต็ม `redux/actions.js`

```js
const Types = {
    GET_USERS_SUCCESS: 'GET_USERS_SUCCESS'
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

export default {
    Types,
    startGetUser
} 
```