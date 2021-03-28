# สร้างกลไกการ save note 

สามารถ clone โปรเจคมาเริ่มทำขั้นตอนนี้ได้จากคำสั่ง git ด้านล่าง

```bash
git clone -b start-for-redux-dispatch https://github.com/teerasej/react-native-note-app-with-redux
```

## Dispatch Action Object ที่มีข้อมูลสำหรับสร้าง note เข้า Redux 

```jsx
// pages/new-note-page/NewNotePage.js

//.. 

// useDispatch() hook เพื่อใช้ส่ง action object เข้า redux
import { useDispatch } from 'react-redux';

export default function NewNotePage({ navigation }) {

  // เรียกใช้ useDispatch() hook จะได้ function ที่ใช้ส่ง Action object เข้าไปที่ redux
  const dispatch = useDispatch();

  //..

  return (

    {//...}

    <Formik
      initialValues={{
          message: '',
      }}
      validationSchema={validationSchema}
      onSubmit={(values, { resetForm  }) => {
          console.log(values)
          resetForm()

          // dispatch object ที่มี type และ payload เข้าไปที่ redux 
          // object นี้จะถูกส่งต่อให้ reducer อีกที
          dispatch({
              type: actions.SAVE_NEW_NOTE,
              payload: values.message
          })

          navigation.goBack()
      }}
    >

     {//...}

  )
}
```

## 3. นำ Note ใหม่ใส่เข้า state ของ reducer

ข้อแม้ของการที่ React component จะ render ตัวเองใหม่จากการได้รับ redux state ใหม่ ก็คือ**ตัว property นั้นต้องเป็น reference หรือ object ใหม่ทั้งหมด **

```js

import actions from './actions'

const initialState = {
    notes: []
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    // ถ้าค่า type ของ action object ที่ส่งมาตรงกับ case เข้าเคสนี้
    case actions.SAVE_NEW_NOTE:

        // แสดงข้อมูลที่ได้รับจาก action object 
        console.log('new note message:', payload)

        // ประกาศตัวแปร array ใหม่
        // ...state.notes --> เป็นการสั่งลอก item ทั้งหมดจาก state.notes ตัวปัจจุบัน มาไว้ที่ array ใหม่นี้
        // [ ...state.notes, { title: payload } ] --> เป็นการสั่งลอก item ทั้งหมดจาก state.notes ตัวปัจจุบัน มาไว้ที่ array ใหม่แล้ว ให้เพิ่ม { title: payload } เข้าไปใน array ใหม่นี้ด้วย
        const newNotesArray = [ ...state.notes, { title: payload } ]

        // สร้าง redux state object ใหม่
        //  ...state --> สั่งลอก property ทุกตัวจากตัวแปร state ปัจจุบัน มาไว้ที่ object ใหม่ 
        // notes: newNotesArray --> กำหนดค่า property notes ของ state object เป็น array ตัวใหม่แทน
        return { ...state, notes: newNotesArray }

    default:
        return state
    }
}

```

### แบบที่จะไม่ทำให้ Component render ใหม่

สังเกตว่าโค้ดด้านล่าง เราแค่เอาค่า redux state ตัวเดิม มาเพิ่มค่าลงลงไปใน array ตัวเดิมเท่านั้น 
หากไม่มีการสร้าง reference ของ object ใหม่ จะไม่ทำให้เกิดการ render ใหม่ใน component ที่ใช้ตัวแปรจาก reduxt state 

```js
case actions.SAVE_NEW_NOTE:
        state.notes.push({title:payload})
        return state
```


## 4. เรียกใช้ค่า redux state ใน HomePage ด้วย useSelector() hook

```js
// pages/home-page/HomePage.js

//..

import { useSelector } from 'react-redux';

export default function HomePage({ navigation }) {

    // function ใน useSelector จะได้รับ redux state ในตอนเริ่ม render ครั้งแรก และทุกครั้งที่ reducer return ค่า redux state object ใหม่ออกมา
    // เราจึงสามารถเลือกค่า property จาก redux state มาเก็บในตัวแปร เพื่อใช้ใน component ได้ 
    // ตัวอย่างนี้ เราดึง property .notes เพื่อเอามาใช้เป็น array แสดงใน List ของหน้า Homepage
    const notes = useSelector(state => state.notes)

    //..

    // ยกเลิกการใช้ตัวแปร notes เดิมที่อยู่ใน component ไปใช้ของ useSelector แทน
    // let notes = [
    //     { title: 'a' },
    //     { title: 'b' },
    //     { title: 'c' }
    // ]

    //.. 

    return (
        <Container>
            <Content>
                <List>
                    {
                        // ตอนนี้ ตัวแปร notes ที่ได้เป็นข้อมูลที่มาจาก redux state object แทน
                        // และเมื่อ reducer อัพเดต redux state object ใหม่ จะทำให้ useSelector ทำงาน เป็นผลให้ Component นี้ render ตัวเองใหม่
                        notes.map((item, index) => {
                            return (
                                <ListItem key={index}>
                                    <Text>{item.title}</Text>
                                </ListItem>
                            )
                        })
                    }
                </List>
            </Content>
        </Container>
    )
}
```

## ไฟล์เต็ม `pages/home-page/HomePage.js`

```jsx
import { Body, Container, Content, Header, List, ListItem, Title, Button, Icon } from 'native-base'
import React, { useLayoutEffect } from 'react'
import { View, Text } from 'react-native'
import { useSelector } from 'react-redux';

export default function HomePage({ navigation }) {

    const notes = useSelector(state => state.notes)

    useLayoutEffect(() => {
        navigation.setOptions({
            headerRight: () => (
                <Button transparent onPress={onPlusButtonPress}>
                    <Icon
                        name='add'
                        style={
                            { color: 'white' }
                        }
                    />
                </Button>
            ),
        });
    }, [navigation]);

    const onPlusButtonPress = () => {
        navigation.navigate('New Note Page')
    }

    return (
        <Container>
            <Content>
                <List>
                    {
                        
                        notes.map((item, index) => {
                            return (
                                <ListItem key={index}>
                                    <Text>{item.title}</Text>
                                </ListItem>
                            )
                        })
                    }
                </List>
            </Content>
        </Container>
    )
}

```

## ไฟล์เต็ม `pages/new-note-page/NewNotePage.js`

```jsx
import { Body, Header, Button, Container, Content, Input, Item, Label, Title } from 'native-base'
import React from 'react'
import { View, Text } from 'react-native'
import * as yup from 'yup'
import { Field, Formik } from "formik";
import { useDispatch } from 'react-redux';
import actions from '../../redux/actions';

export default function NewNotePage({ navigation }) {

    const dispatch = useDispatch();

    const validationSchema = yup.object({
        message: yup.string()
            .required('Please fill something')
            .min(3, 'fill something more...')
    })


    return (
        <Container>
            <Content padder>
                <Formik
                    initialValues={{
                        message: '',
                    }}
                    validationSchema={validationSchema}
                    onSubmit={(values, { resetForm  }) => {
                        console.log(values)
                        resetForm()
                        dispatch({
                            type: actions.SAVE_NEW_NOTE,
                            payload: values.message
                        })
                        navigation.goBack()
                    }}
                >
                    {
                        ({ handleChange, handleBlur, handleSubmit, values, errors }) => {
                            return (
                                <View>
                                    <Item stackedLabel >
                                        <Label>Message: </Label>
                                        <Field
                                            id="message"
                                            name="message"
                                            component={Input}
                                            value={values.message || ''}
                                            onChangeText={handleChange('message')}
                                            onBlur={handleBlur('message')}
                                        />
                                        {Boolean(errors.message) ?
                                            (<Text>{errors.message}</Text>) : null
                                        }
                                    </Item>
                                    <Button
                                        block
                                        primary
                                        onPress={handleSubmit}
                                    >
                                        <Text>Save</Text>
                                    </Button>
                                </View>
                            )
                        }
                    }
                </Formik>
            </Content>
        </Container>
    )
}

```