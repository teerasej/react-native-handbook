
# กำหนดประเภทของ Action (Action Type)

สร้างไฟล์ `redux/action.js`

เรากำหนดชื่อ action type แต่ละแบบแยกออกมาเพื่ออ้างอิงใน 2 ส่วน

- **ใน reducer:** เพื่อเทียบ type ของ action เพื่อให้อัพเดต redux state ได้ตามต้องการ
- **ใน component:** เพื่อให้กำหนด type ของ action ในการส่งข้อมูลไปหา reducer

```js
export default {
    ADD: 'ADD_NUMBER'
}
```