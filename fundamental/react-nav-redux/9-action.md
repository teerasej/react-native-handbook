
# 9. สร้าง Action ที่เกิดขึ้นในระบบ

สร้างไฟล์ `redux/actions.js`

```js
// กำหนด object ซึ่งเก็บชื่อ Action แบบต่างๆ เอาไว้
const Types = {
    SAVE_NEW_NOTE: 'SAVE_NEW_NOTE'
}

// สร้าง function ที่ใช้ในการกำหนดข้อมูลของ Action 
const createNewNote = (message) => {
    return {
        type: Types.SAVE_NEW_NOTE,
        payload: message
    };
}

// เสร็จแล้วอย่าลืม Export เพื่อเอาไปใช้ในไฟล์อื่นๆ
export default {
    Types,
    createNewNote
} 
```