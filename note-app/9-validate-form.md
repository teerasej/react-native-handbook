
# 10. สร้าง validate function และเอาไปใช้กับ redux form

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

## 2. ใช้ Alert ในการแจ้งเตือนในกรณีที่เกิด Error

```jsx
import { View, Alert } from 'react-native'

renderInput({ input, label, type, meta: { touched, error, warning } }) {
        var hasError = false;

        // ถ้า function ได้รับ error ก็จะแสดง pop up ขึ้นมา
        if (error !== undefined && touched) {
            hasError = true;
            Alert.alert(
                'Opps',
                error
            );
        }
        return (
                <Item error={hasError}>
                    <Input {...input} />
                </Item>
        )
    }
```
