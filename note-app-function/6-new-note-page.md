
# 6. สร้างหน้า New Note Page

## 1. สร้างไฟล์ NewNotePage.js

```jsx
// pages/new-note-page/NewNotePage.js
// snippet: rnf
import React from 'react'
import { View, Text } from 'react-native'

export default function NewNotePage() {
    return (
        <View>
            <Text>New Note Page</Text>
        </View>
    )
}
```

เสร็จแล้วเอาไปแทนที่ `<HomePage/>` ในไฟล์ `App.js` เป็นการชั่วคราว

```jsx
// App.js

export default function App() {
    
    //...
    
    return (
        <NewNotePage/>
    )
}
```



## 2. สร้างส่วนของ UI ด้วย Nativebase

```jsx
// pages/new-note-page/NewNotePage.js

import { Body, Header, Button, Container, Content, Input, Item, Label, Title } from 'native-base'

import React from 'react'
import { View, Text } from 'react-native'

export default function NewNotePage() {

    return (
        <Container>
            <Header>
                <Body>
                    <Title>New Note</Title>
                </Body>
            </Header>
            <Content padder>
                {/* สร้าง Label และ Field */}
                <Item stackedLabel>
                    <Label>Message: </Label>
                    <Input placeholder="Message" />
                </Item>
                {/* ปุ่มยืนยันการสร้าง Note */}
                <Button block primary>
                    <Text>Save</Text>
                </Button>
            </Content>
        </Container>
    )
}



```

## 3. สร้าง Validation Schema ด้วย Yup สำหรับตรวจสอบข้อมูลจาก Form


```jsx
// pages/new-note-page/NewNotePage.js

// ...
import * as yup from 'yup'

export default function NewNotePage() {

    // ใช้ yup ในการกำหนด rule ให้กับแต่ละ field
    const validationSchema = yup.object({

        // .string() กำหนดให้ field ชื่อ message เก็บค่า string
        // .required('...'): ต้องมีการกรอกข้อมูล ถ้าไม่มีให้แสดงข้อความตามที่กำหนด
        // .min(3,'...'): ต้องมีการกรอกอย่างน้อย 3 ตัวอักษร ถ้าไม่มีให้แสดงข้อความตามที่กำหนด
        message: yup.string()
            .required('Please fill something')
            .min(3,'fill something more...')
    })
```

## 4. กำหนดค่าเริ่มต้น Formik และ Field component ในการควบคุมการรับข้อมูลจาก Form

- Formik เป็น JSX Component  รูปแบบหนึ่ง เราสามารถสร้าง tag ของ Formik มาครอบส่วนที่ต้องการใช้งานกับ Form ได้ และมีการกำหนดค่า props ต่างๆ เพื่อใช้ในกระบวนการรับข้อมูลจาก form
- `<Field>` เป็น JSX component ที่ formik สร้างขึ้นเพื่อการทำงานร่วมกับ JSX Component จาก UI Kit อื่นๆ ในที่นี้เราจะเอามาใช้กับ `<Input>` ของ Nativebase 

```jsx
// pages/new-note-page/NewNotePage.js

// ...
import { Field, Formik } from "formik";

export default function NewNotePage() {

    //...

    return (
        <Container>
            <Header>
                <Body>
                    <Title>New Note</Title>
                </Body>
            </Header>
            <Content padder>
            {/* 
                เรียกใช้ Formik component 
                - initialValues:  กำหนดค่าเริ่มต้นของข้อมูลแต่ละ field
                - validationSchema: yup object ในการ validate ข้อมูล
                - onSubmit: function ที่จะทำงานถ้ามีการเรียกใช้ function handleSubmit ของ Formik
            */}
            <Formik
                    initialValues={{
                        message: '',
                    }}
                    validationSchema={validationSchema}
                    onSubmit={(values, { resetForm  }) => {
                        console.log(values)
                        resetForm()
                    }}
                >
                   
                    <Item stackedLabel>
                        <Label>Message: </Label>
                        {/* 
                            นำ Field component ของ formik มาใช้กับ <Input> ของ Nativebase
                            - การกำหนด id และ name ให้ตรงกับชื่อข้อมูลที่กำหนดไว้ใน initialValues ของ Formik จะทำให้ formik สามารถจับคู่ Field กับตัวข้อมูลได้อัตโนมัติ
                            - props ชื่อ component เราจะกำหนด Input ลงไปเพื่อให้เราได้หน้าตาของ Input เหมือนเดิม เพิ่มเติมคือทำงานกับ Formik ได้
                        */}
                        <Field
                            id="message"
                            name="message"
                            component={Input}
                        />
                    </Item>
                 
                    <Button block primary>
                        <Text>Save</Text>
                    </Button>
                   
                </Formik>
            </Content>
        </Container>
    )
}
```

## 5. ใช้ formik object ในการสร้างแบบฟอร์ม ที่นำตัวแปรและ function ต่างๆ ของ formik มาใช้

```jsx
// pages/new-note-page/NewNotePage.js

// ...
import { Field, Formik } from "formik";

export default function NewNotePage() {

    //...

    return (
        <Container>
            <Header>
                <Body>
                    <Title>New Note</Title>
                </Body>
            </Header>
            <Content padder>
                <Formik
                    initialValues={{
                        message: '',
                    }}
                    validationSchema={validationSchema}
                    onSubmit={(values, { resetForm  }) => {
                        console.log(values)
                        resetForm()
                    }}
                >
                    {/* 
                        สร้าง function ที่รับ formik object เข้ามาใช้งานกับ component ต่างๆ 
                        ปกติ เราสามารถเขียนรับ parameter ชื่อ formik ได้โดยตรง แบบนี้ 

                        (formik) => { }

                        แต่เราสามารถใช้เทคนิคการเขียนที่ชื่อ deconstructing object เพื่อเลือกเฉพาะ property ที่ต้องการของ object formik มาใช้ ในที่นี้ได้แก่

                        - handleChange(''), handleBlur(''): นำมาใช้กับ <Field> โดยระบุชื่อของ property ที่ต้องการให้จัดการ มีหน้าที่ทำให้กลไกการทำงานของการกรอกข้อมูลใน Field เป็นอย่างที่ควรจะเป็น
                        
                        - handleSubmit: มักนำมาใช้กับ props ต่างๆ ในปุ่มกด เช่น onPress 

                        - values: ปกติจะนำมาใช้กับ <Field> ในการจัดการค่า values เช่น

                        <Field values={values.message || ''}>

                        ** แต่ถ้ามีการกำหนด props ชื่อ id และ name ตรงกับข้อมูลใน formik object ก็ไม่จำเป็นต้องเขียนส่วนนี้ก็ได้

                        ** ในกรณีที่ต้องการทำงานร่วมกับ resetForm() ต้องมีการเขียนดึงค่า value.{ชื่อข้อมูล} มาใช้โดยตรงดังตัวอย่างด้านบน

                        - errors: เก็บค่า error ของแต่ละ field ที่ได้จากการ validate ไว้ ส่วนนี้จะทำงานร่วมกับ validationSchema ของ formik component ซึ่ง property จะอิงตามชื่อของข้อมูลที่กำหนดไว้ใน initialValues ของ formik

                        ในกรณีที่ validate ข้อมูลไม่มีปัญหา ค่า property นั้นจะเป็น null หรือ undefined
                        ในกรณีที่ validate ข้อมูลแล้วไม่ถูกต้อง ค่า property นั้นจะเป็น string ที่มักเป็นข้อความ error จากการ validate
                    */}
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

## ไฟล์เต็ม `pages/new-note-page/NewNotePage.js`

```jsx
import { Body, Header, Button, Container, Content, Input, Item, Label, Title } from 'native-base'
import React from 'react'
import { View, Text } from 'react-native'
import * as yup from 'yup'
import { Field, Formik } from "formik";

export default function NewNotePage() {

    const validationSchema = yup.object({
        message: yup.string()
            .required('Please fill something')
            .min(3, 'fill something more...')
    })


    return (
        <Container>
            <Header>
                <Body>
                    <Title>New Note</Title>
                </Body>
            </Header>
            <Content padder>
                <Formik
                    initialValues={{
                        message: '',
                    }}
                    validationSchema={validationSchema}
                    onSubmit={(values, { resetForm  }) => {
                        console.log(values)
                        resetForm()
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

