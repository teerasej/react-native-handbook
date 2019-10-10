# 8. เรียกใช้ค่าจาก redux state ใน HomePage

## 1. กำหนดค่าใน initial state 

เปิดไฟล์ `redux/note.reducer.js`

เราจะกำหนดค่าชื่อ `notes` ลงไปใน object `initialState` 

```js
const initialState = {

    notes: [
        { title: 'a' },
        { title: 'b' },
        { title: 'c' }
    ]
}

export default (state = initialState, { type, payload }) => {
```

## 2. เรียกใช้ redux state object ผ่าน function `mapStateToProp`

เปิดไฟล์ `pages/home-page/HomePage.js`

ฟังก์ชั่น `mapStateToProps` จะได้รับค่า `state` จาก reducer ซึ่งเราสามารถเรียกใช้ค่าเหล่านี้ใน component ได้ ผ่านการกำหนดชื่อ property

ซึ่งชื่อ property พวกนี้จะสามารถเรียกใช้ได้ผ่าน `this.props` ใน component

```js
const mapStateToProps = (state) => ({
  // ค่าที่จะเรียกใช้จาก this.props : ค่าที่ได้จาก redux state ที่ส่งจาก reducer  
    notes: state.note.notes
})
```

## 3. แก้ไขส่วน render() ของ HomePage ให้ดึงค่าจาก this.props มาใช้

```jsx
render() {
        return (

            <Content>
                <List>
                    <ListItem>
                        <Text>A</Text>
                    </ListItem>
                    <ListItem>
                        <Text>B</Text>
                    </ListItem>
                    {
                        this.props.notes.map((item, index) => {
                            return (
                                <ListItem key={index}>
                                    <Text>{item.title}</Text>
                                </ListItem>
                            )
                        })
                    }
                </List>
            </Content>
```

## A. ไฟล์เต็ม `pages/home-page/HomePage.js`

```jsx
import React, { Component } from 'react'
import { View } from 'react-native'
import { connect } from 'react-redux'
import { Container, Header, Title, Content, List, ListItem, Text, Left, Right, Body, Button, Icon } from 'native-base';


export class HomePage extends Component {

    static navigationOptions = ({ navigation }) => {
        return {
            title: 'Home',
            headerRight: (
                <Button transparent
                    onPress={() => navigation.navigate('CreateNote')}
                >
                    <Icon name='add' />
                </Button>
            )
        }
    };



    render() {
        return (

            <Content>
                <List>
                    {
                        this.props.notes.map((item, index) => {
                            return (
                                <ListItem key={index}>
                                    <Text>{item.title}</Text>
                                </ListItem>
                            )
                        })
                    }
                </List>
            </Content>

        )
    }
}

// snippet: reduxmap
const mapStateToProps = (state) => ({
    notes: state.note.notes
})

const mapDispatchToProps = {

}


export default connect(mapStateToProps, mapDispatchToProps)(HomePage);
```