# 10. สร้างกลไกการ save note 

## 1. สร้าง action และ dispatch note ใหม่เข้า store

Redux Action ส่วนใหญ่จะถูกส่งจาก Component มายัง reducer

สร้างไฟล์​

```js
// redux/actions.js

const ActionTypes = {
    SAVE_NEW_NOTE: 'SAVE_NEW_NOTE'
}

const saveNewNote = (message) => (
    {
        type: ActionTypes.SAVE_NEW_NOTE,
        payload: message
    }
)

export default {
    ActionTypes,
    saveNewNote
} 
```

### Action

_เปรียบได้คล้ายๆ กับ Event ใน MVC แต่ Action จะวิ่งระหว่างส่วนต่างๆ ของ Redux_

ไฟล์กลุ่มแรกคือ **Actions** จะมีส่วนประกอบใหญ่ๆ อยู่ 2 ส่วนคือ 

1. **Action Type** เป็นประเภทของ Action คล้ายๆ ค่า Enum เอาไว้อ้างอิงในส่วนอื่นๆ ของ Redux
2. **Action Object** ทำหน้าที่คล้ายๆ พัศดุที่เก็บข้อมูล (ข้อมูลส่วนนี้ เรียกทั่วไปว่า payload) ตัว Action Object มักจะถูกส่งจาก Component เพื่อเดินทางไปยัง Reducer

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

const mapDispatchToProps = dispatch => {
  return {
    saveNewNote: (message) => dispatch(actions.saveNewNote(message))
  }
}



export default connect(mapStateToProps, mapDispatchToProps)(NewNotePage)
```

## 3. เขียน Test Unit ให้กับ note.reducer.js

### Setup Test Environment

- [ทำการติดตั้ง Test Environment](test/1-setup.md)


สร้างไฟล​์​ `redux/reducers/note.reducer.test.js`

```js

import reducer from "./note.reducer";
import actions from "./../actions";

describe('Note Reducer', () => {
    
    it('Initial Note should be empty', () => {
        expect(reducer(undefined, {}).notes).toEqual([]);
    });

    it('Notes should have new item', () => {
        const action = {
            type: actions.ActionTypes.SAVE_NEW_NOTE,
            payload: 'hello'
        }

        const notes = reducer(undefined, action).notes;

        expect(notes.length).toBe(1);
        expect(notes[0]).toStrictEqual({ title: 'hello'});
    });

    it('Notes should have 2 new item', () => {
        const action1 = {
            type: actions.ActionTypes.SAVE_NEW_NOTE,
            payload: 'hello'
        }

        const action2 = {
            type: actions.ActionTypes.SAVE_NEW_NOTE,
            payload: 'test'
        }

        const state = reducer(undefined, action1);
        const finalState = reducer(state, action2);

        expect(finalState.notes.length).toBe(2);
        expect(finalState.notes[0]).toStrictEqual({ title: 'hello'});
        expect(finalState.notes[1]).toStrictEqual({ title: 'test'});
    });

});
```

## 3. นำ Note ใหม่ใส่เข้า state ของ reducer

```js
import actions from "../actions";

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

const mapStateToProps = (state) => ({
    notes: state.note.notes
})

```
