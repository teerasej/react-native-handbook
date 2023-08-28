
# 9. สร้าง Action ที่เกิดขึ้นในระบบ

สร้างไฟล์ `redux/actions.js`

```js
// กำหนด object ซึ่งเก็บชื่อ Action แบบต่างๆ เอาไว้
export const actionTypes = {
    SAVE_NEW_NOTE: 'SAVE_NEW_NOTE'
}

// สร้าง function ที่ใช้ในการกำหนดข้อมูลของ Action 
export const createNewNote = (message) => {
    return {
        type: actionTypes.SAVE_NEW_NOTE,
        payload: message
    };
}

```