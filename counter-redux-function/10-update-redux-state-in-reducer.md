
# กำหนดให้ reducer จัดการ Action และ payload 

```js
// redux/reducer.js

import actions from "./actions"

const initialState = {
    count: 0
}

// หากมี action ใหม่ส่งให้ reducer 
// ทาง redux store จะส่ง state object ปัจจุบันเข้ามาใน function ด้วย 
// เพื่อให้ เราสามารถอ้างอิงข้อมูลปัจจุบันใน redux state ปัจจุบันได้ ก่อนที่จะใช้มันสร้าง reduxt state object ใหม่
export default (state = initialState, { type, payload }) => {
    switch (type) {

    // ถ้าค่า type ของ action object ที่ส่งมาตรงกับ case เข้าเคสนี้
    case actions.ADD:

        // แสดงข้อมูลที่ได้รับจาก action object 
        console.log(`adding ${payload} to count: ${state.count}`, )
        
        // ประกาศตัวแปร count ใหม่
        // โดยการนำ ค่า count ใน state ปัจจุบัน (state.count) มาบวกด้วยค่า payload
        let newCount = state.count + payload
        
        // แสดงค่า count ใหม่ที่จะกำหนดลงไปใน state object ใหม่
        console.log('new count:', newCount)
        
        // สร้าง redux state object ใหม่
        //  ...state --> สั่งลอก property ทุกตัวจากตัวแปร state ปัจจุบัน มาไว้ที่ object ใหม่ 
        // count: newCount --> กำหนดค่า property count ของ state object ค่าใหม่แทน
        return { ...state, count: newCount }

    default:
        return state
    }
}

```