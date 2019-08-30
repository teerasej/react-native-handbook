# 10. สร้างกลไกการ save note 

## 1. สร้าง action และ dispatch note ใหม่เข้า store

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