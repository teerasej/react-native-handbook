
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

## 2. สร้างส่วนของ UI ด้วย Nativebase

```jsx
import { Body, Container, Title } from 'native-base'
import React from 'react'
import { View, Text } from 'react-native'
import { Header } from 'react-native/Libraries/NewAppScreen'

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
                    <Field name="message"  />
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

## 3. แยกส่วน Form ไปไว้ใน Component ของตัวเอง เพื่อใช้กับ redux-form

```jsx

```

## 4. ปรับหน้า NewNotePage ให้ใช้ NewNoteForm 

```jsx

```
## 4.A ไฟล์เต็ม `pages/new-note-page/NewNotePage.js`

```jsx
import React, { Component } from 'react'
import { View } from 'react-native'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'
import { Container, Header, Title, Content, List, ListItem, Text, Left, Right, Body, Button, Item, Input, Label } from 'native-base';

import NewNoteForm from './NewNoteForm';

export class NewNotePage extends Component {
  static propTypes = {

  }

  onFormSave = (values) => {
    console.log(values);
  }


  render() {
    return (
            <Container>
                <Header>
                    <Body>
                        <Title>New Note</Title>
                    </Body>
                </Header>
                <Content padder>
                 {/*
                    <NewNoteForm onSubmit={this.onFormSave}/>
                    */}
                </Content>
            </Container>
      
    )
  }
}

const mapStateToProps = (state) => ({

})

const mapDispatchToProps = {

}



export default connect(mapStateToProps, mapDispatchToProps)(NewNotePage)

```

## 5. แสดง NewNotePage ในหน้าแอพ

```jsx
// App.js

import  NewNotePage  from './pages/new-note-page/NewNotePage';

render() {
    return (
      
      <NewNotePage />
    
    )
}
```
