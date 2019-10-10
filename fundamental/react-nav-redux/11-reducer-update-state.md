

# 11. กำหนด Action Type ที่ Reducer ต้องเอาข้อมูลมาอัพเดตใน redux state

เปิดไฟล์ `redux/note.reducer.js`

เราสามารถกำหนด switch case ที่เทียบค่า type ของ action ที่ส่งเข้ามาใน reducer ได้ 

```js
import actions from "../redux/actions";

export default (state = initialState, { type, payload }) => {
    switch (type) {

    // เทียบค่า type ของ action 
    case actions.Types.SAVE_NEW_NOTE:

        // ถ้าสำเร็จ เราจะสร้าง redux state object ใหม่ 
        // โดยการโอนค่าเก่าจาก redux state object เดิมลงมาด้วย โดยใช้คำสั่ง ...state 
        // และสำหรับ array ต้องใช้แบบเดียวกัน 
        return { ...state, notes: [...state.notes, { title: payload }] }
```

## A. ไฟล์เต็ม `redux/note.reducer.js`

```js
import actions from "../redux/actions";

const initialState = {
    notes: [
        { title: 'a' },
        { title: 'b' },
        { title: 'c' }
    ]
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case actions.Types.SAVE_NEW_NOTE:
        return { ...state, notes: [...state.notes, { title: payload }] }

    default:
        return state
    }
}
```