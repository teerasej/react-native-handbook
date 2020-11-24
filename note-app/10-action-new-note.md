# 10. สร้างกลไกการ save note 

สามารถ clone โปรเจคมาเริ่มทำขั้นตอนนี้ได้จากคำสั่ง git ด้านล่าง

```bash
git clone -b start-for-redux-dispatch https://github.com/teerasej/react-native-note-app-with-redux
```

## 1. สร้าง action และ dispatch note ใหม่เข้า store

Redux Action ส่วนใหญ่จะถูกส่งจาก Component มายัง reducer

สร้างไฟล์​

```js
// redux/actions.js

const ActionTypes = {
    SAVE_NEW_NOTE: 'SAVE_NEW_NOTE'
}

export default {
    ActionTypes
} 
```

### Action

_เปรียบได้คล้ายๆ กับ Event ใน MVC แต่ Action จะวิ่งระหว่างส่วนต่างๆ ของ Redux_

ไฟล์กลุ่มแรกคือ **Actions** จะมีส่วนประกอบใหญ่ๆ อยู่ 2 ส่วนคือ 

1. **Action Type** เป็นประเภทของ Action คล้ายๆ ค่า Enum เอาไว้อ้างอิงในส่วนอื่นๆ ของ Redux
2. **Action Payload** ทำหน้าที่คล้ายๆ พัศดุที่เก็บข้อมูล (ข้อมูลส่วนนี้ เรียกทั่วไปว่า payload) ตัว Action Object มักจะถูกส่งจาก Component เพื่อเดินทางไปยัง Reducer

## 2. เรียกใช้ Action ใน dispatch ของ NewNotePage 

```jsx
// pages/new-note-page/NewNotePage.js

import React, { Component } from 'react'
import { View } from 'react-native'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'
import { Container, Header, Title, Content, List, ListItem, Text, Left, Right, Body, Button, Item, Input, Label } from 'native-base';

import NewNoteForm from './NewNoteForm';
import actions from '../../redux/actions';

export class NewNotePage extends Component {
  static propTypes = {

  }

  static navigationOptions = {
    title: 'New Note'
  };

  onFormSave = (values) => {
    console.log(values);
    
    // เรียกใช้ dispatch function ของ props
    this.props.saveNewNote(values.message);
    
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

// สร้าง dispatch property ในรูปแบบของ function ที่ส่ง object dispatch ได้
// { type: 'action name', payload: any }
const mapDispatchToProps = dispatch => {
  return {
    saveNewNote: (message) => dispatch({ type: actions.ActionTypes.SAVE_NEW_NOTE, payload: message })
  }
}



export default connect(mapStateToProps, mapDispatchToProps)(NewNotePage)
```

## (Optional) เขียน Test Unit ให้กับ Reducer

[ดูเพิ่มเติมได้ที่นี่](10-1-test-note-reducer.md)


## 3. นำ Note ใหม่ใส่เข้า state ของ reducer

ข้อแม้ของการที่ Redux component จะ setState ตัวเองใหม่จากการได้รับ Props ก็คือตัว props นั้นต้องเป็น reference หรือ object ใหม่ทั้งหมด 

```js
import actions from "../actions";

const initialState = {
    notes: []
}


export default (state = initialState, { type, payload }) => {
    switch (type) {

    case actions.ActionTypes.SAVE_NEW_NOTE:
        return { ...state, notes: [ ...state.notes, { title: payload }] }

    default:
        return state
    }
}
```


## 4. และ นำมาใช้ใน HomePage

```js
// pages/home-page/HomePage.js

const mapStateToProps = (state) => {
    return {
      notes: state.note.notes
    }
}

```

หรือจะเขียนในรูปแบบย่อแบบด้านล่างก็ได้ 

```js
// pages/home-page/HomePage.js

const mapStateToProps = (state) => ({
    notes: state.note.notes
})

```
