
# 10. กำหนด function ที่ต้องการสร้าง Action ใน NewNotePage

เปิดไฟล์ `pages/new-note-page/NewNotePage.js` 

เนื่องจาก action ชื่อ `SAVE_NEW_NOTE` เป็น action ที่เราจะกำหนดให้เกิดขึ้นตอนผู้ใช้กรอกข้อมูลในหน้า `NewNotePage` และกดปุ่ม **Save** 

เราจึงสามารถเรียกใช้ react hook ชื่อ `useDispatch` เพื่อใช้ในการส่ง action object ไปที่ reducer

```jsx
// import action ที่เขียนไว้
import { actionTypes, createNewNote } from '../../redux/actions';
```

สุดท้ายเราเรียกใช้ `createNewNote(message)` ใน function `onSubmit` ผ่านตัวแปร

```js
<Formik
    initialValues={{ message: '' }}
    onSubmit={
      values => {
        console.log(values)

        dispatch(createNewNote(values.message));
        // dispatch({
        //   type: actionTypes.SAVE_NEW_NOTE,
        //   payload: values.message
        // })

        props.navigation.goBack()
      }
    }
  >
```

## A. ไฟล์เต็ม `pages/new-note-page/NewNotePage.js` 

```jsx
import React, { Component } from 'react'
import { View, TextInput } from 'react-native'
import {  Content, Text, Button, Item, Input, Label } from 'native-base';
import { Formik } from 'formik';
import { actionTypes, createNewNote } from '../../redux/actions';
import { useDispatch } from 'react-redux';

export default function NewNotePage(props) {

  const dispatch = useDispatch()

  return (
    <Content padder>
      <View>
        <Formik
          initialValues={{ message: '' }}
          onSubmit={
            values => {
              console.log(values)
              dispatch(createNewNote(values.message));
              // dispatch({
              //   type: actionTypes.SAVE_NEW_NOTE,
              //   payload: values.message
              // })
              props.navigation.goBack()
            }
          }
        >
          {({ handleChange, handleBlur, handleSubmit, values }) => (
            <View>
              <Item inlineLabel >
                <Label>Message: </Label>
                <Input
                  onChangeText={handleChange('message')}
                  onBlur={handleBlur('message')}
                  value={values.message}
                />
              </Item>
              <Button block primary onPress={handleSubmit} >
                <Text>Save</Text>
              </Button>
            </View>
          )}
        </Formik>
      </View>
    </Content>
  )
}


```