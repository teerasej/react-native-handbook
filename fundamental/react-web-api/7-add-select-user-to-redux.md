
# 7. เพิ่ม user ที่ถูกเลือกเข้า redux state

เปิดไฟล์ `redux/app.reducer.js`

```js
import actions from "./actions";

const initialState = {
    
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case actions.Types.GET_USERS_SUCCESS:
        return { ...state, users: [ ...payload] }

    // เพิ่มกรณีเกิด action USER_SELECTED
    case actions.Types.USER_SELECTED: 
        // ตั้งชื่อค่า payload เป็น selectedUser
        return { ...state, selectedUser: payload }

    default:
        return state
    }
}
```