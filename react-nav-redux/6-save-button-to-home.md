
# 6. กำหนดปุ่ม Save ให้มีการย้อนกลับไปหน้าแรก

เปิดไฟล์ `pages/new-note-page/NewNotePage.js`

ฟังก์ชั่น `onFormSave` จะทำงานตอนกดปุ่ม Save ตรงนี้เราจะเรียกใช้ navigation เพื่อย้อนกลับไปหน้าแรก

```js
  onFormSave = (values) => {
    console.log(values);

    this.props.navigation.goBack();
  }
```