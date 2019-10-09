
# 10. กำหนด function ที่ต้องการสร้าง Action ใน NewNotePage

เปิดไฟล์ `pages/new-note-page/NewNotePage.js` 

เนื่องจาก action ชื่อ `SAVE_NEW_NOTE` เป็น action ที่เราจะกำหนดให้เกิดขึ้นตอนผู้ใช้กรอกข้อมูลในหน้า `NewNotePage` และกดปุ่ม **Save** 

เราจึงสามารถกำหนด ชื่อ function ขึ้นมาใน `mapDispatchToProps` เพื่อให้สามารถเรียกใช้ใน component ได้ ผ่าน `this.props`

```jsx
// import action ที่เขียนไว้
import actions from "./../../redux/actions";

// กำหนด function ชื่อ createNewNote
// ซึ่ง function นี้รับ parameter ชื่อ message และเรียกใช้คำสั่ง dispatch เพื่อส่ง action เข้าไปในระบบ
const mapDispatchToProps = (dispatch) => {
  return {
    createNewNote: (message) => dispatch(actions.createNewNote(message))
  }
}
```

สุดท้ายเราเรียกใช้ `createNewNote(message)` ใน function `onFormSave` ผ่านตัวแปร `this.props`

```js
onFormSave = (values) => {
    console.log(values);
    this.props.createNewNote(values.message);
    this.props.navigation.goBack();
  }
```

## A. ไฟล์เต็ม `pages/new-note-page/NewNotePage.js` 

```jsx
import React, { Component } from 'react'
import { View } from 'react-native'
import { connect } from 'react-redux'
import { Container, Header, Title, Content, List, ListItem, Text, Left, Right, Body, Button, Item, Input, Label } from 'native-base';
import actions from "./../../redux/actions";
import NewNoteForm from './NewNoteForm';

export class NewNotePage extends Component {
  
  static navigationOptions = {
    title: 'New Note'
  };

  onFormSave = (values) => {
    console.log(values);
    this.props.createNewNote(values.message);
    this.props.navigation.goBack();
  }


  render() {
    return (
      <Content padder>
        <NewNoteForm onSubmit={this.onFormSave}/>
      </Content>
    )
  }
}

const mapStateToProps = (state) => ({
  
})

const mapDispatchToProps = (dispatch) => {
  return {
    createNewNote: (message) => dispatch(actions.createNewNote(message))
  }
}


export default connect(mapStateToProps, mapDispatchToProps)(NewNotePage)
```