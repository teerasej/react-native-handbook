
# 9. สร้าง validate function และเอาไปใช้กับ redux form

- [Redux Form](https://redux-form.com/)

## 1. สร้าง validate function ที่จะรับค่าจาก redux form

โดยในที่นี้ function จะส่งค่า error object ออกไป 

```js
// pages/new-note-page/NewNoteForm.js

const validate = values => {

    const error = {};
    error.message = '';

    var newMessage = values.message;

    if (values.message === undefined) {
        newMessage = ''
    }

    if (newMessage === '') {
        error.message = 'fill something';
    } 

    return error;
};

// เสร็จแล้วเอามาใส่ใน reduxForm() ด้านล่าง
NewNoteForm = reduxForm({ form: 'newNote', validate })(NewNoteForm)
```


