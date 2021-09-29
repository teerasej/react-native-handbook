
# 6. กำหนดปุ่ม Save ให้มีการย้อนกลับไปหน้าแรก

เปิดไฟล์ `pages/new-note-page/NewNotePage.js`

ฟังก์ชั่น `onSubmit` จะทำงานตอนกดปุ่ม Save ตรงนี้เราจะเรียกใช้ navigation เพื่อย้อนกลับไปหน้าแรก

```js
// กำหนดเรียกใช้ props paremeter ของ function component
export default function NewNotePage(props) {

<Formik
  initialValues={{ message: '' }}
  onSubmit={
    values => {
      console.log(values)

      // เรียกใช้ navigation จาก props
      props.navigation.goBack()
    }
  }
>
```

## ไฟล์เต็ม 

```js
// pages/new-note-page/NewNotePage.js

import React, { Component } from 'react'
import { View, TextInput } from 'react-native'
import {  Content, Text, Button, Item, Input, Label } from 'native-base';
import { Formik } from 'formik';


export default function NewNotePage(props) {
  return (
    <Content padder>
      <View>
        <Formik
          initialValues={{ message: '' }}
          onSubmit={
            values => {
              console.log(values)
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