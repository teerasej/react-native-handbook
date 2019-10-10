
# 10. สร้าง action function สำหรับใช้โทร

เปิดไฟล์ `redux/actions.js`

```js
// import คำสั่งจาก module
import call from 'react-native-phone-call'

// สร้าง function ที่รับค่าเบอร์โทร และเรียกใช้คำสั่งโทรออก
const makeCall = (phoneNumber) => {
    call({number: phoneNumber}).catch(console.error);
}

// อย่าลืม export เอาไปใช้งาน 
export default {
    Types,
    startGetUser,
    selectUser,
    makeCall
} 
```

## A. ไฟล์เต็ม `redux/actions.js`

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