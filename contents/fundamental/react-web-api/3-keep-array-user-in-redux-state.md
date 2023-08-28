
# 3. ส่ง array ที่ได้จาก action ลง redux state

เปิดไฟล์ `redux/app.reducer.js`

เราจะเพิ่ม case ของ action ที่มี type เป็น `actions.Types.GET_USERS_SUCCESS` ลงไปใน Reducer

```js
case actionTypes.GET_USERS_SUCCESS:
    // เพิ่ม payload (array ที่ได้จาก web api) ลง redux state ในชื่อ **users**
    return { ...state, users: [ ...payload] }
```

## A. ไฟล์เต็ม `redux/app.reducer.js`

```js
import { actionTypes } from "./actions"


const initialState = {
    
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case actionTypes.GET_USERS_SUCCESS:
        return { ...state, users: [ ...payload] }

    default:
        return state
    }
}

```