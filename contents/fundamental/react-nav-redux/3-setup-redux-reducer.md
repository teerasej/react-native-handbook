
# 3. สร้าง Redux Reducer

สร้างไฟล์ `redux/reducer.js`

```js
// ใช้ snippet: rxreducer
const initialState = {

}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case '':
        return { ...state, ...payload }

    default:
        return state
    }
}
```

## Reducer

_เปรียบได้คล้ายๆ กับ Controller ใน MVC แต่ Reducer จะส่ง State ให้กับ Component_

_มองว่า State ตอนนี้ ถูกใช้แทน Model ของ MVC ก็ได้_

Reducer เป็นส่วนที่จะจัดการรับ Action ที่ส่งมาจากส่วนต่างๆ ของ Redux, ทำตาม business logic และอัพเดต State กลับไปที่ Component ที่ใช้งาน

reducer ต้องการ ข้อมูลเริ่มต้น สำหรับใช้ส่งไปให้ Component ต่างๆ เสมอ เรามักเรียกส่วนนี้ว่า **initialState**

**initialState** ก็คือ object ตัวหนึ่งที่ reducer จะส่งไปให้กับ Component ต่างๆ ตอนเริ่มทำงานนั่นเอง

เราสามารถกำหนดอะไรลงไปใน **initialState** ก็ได้ เหมือนเป็น object ของ JavaScript ทั่วไป