
# 10. สร้าง action function สำหรับใช้โทร

เปิดไฟล์ `redux/actions.js`

```js
// import คำสั่งจาก module
import call from 'react-native-phone-call'

// สร้าง function ที่รับค่าเบอร์โทร และเรียกใช้คำสั่งโทรออก
export const makeCall = (phoneNumber) => {
    
    let pattern = '[-()]'
    let regex = new RegExp(pattern,'g')
    let phoneNumberClean = phoneNumber.replace(regex,'')

    call({number: phoneNumberClean}).catch(console.error)
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

export const makeCall = (phoneNumber) => {
    
    let pattern = '[-()]'
    let regex = new RegExp(pattern,'g')
    let phoneNumberClean = phoneNumber.replace(regex,'')

    call({number: phoneNumberClean}).catch(console.error)
}

```