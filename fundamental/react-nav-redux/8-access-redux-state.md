# 8. เรียกใช้ค่าจาก redux state ใน HomePage

## 1. กำหนดค่าใน initial state 

เปิดไฟล์ `redux/reducer.js`

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

## 2. เรียกใช้ redux state object ผ่าน react hook `useReducer`

เปิดไฟล์ `pages/home-page/HomePage.js`

ฟังก์ชั่น `useReducer` จะได้รับค่า `state` จาก reducer ซึ่งเราสามารถเรียกใช้ค่าเหล่านี้ใน component ได้ ผ่านการกำหนดชื่อ property


```js
// เรียกใช้ property .notes จาก state object ของ reducer 
const notes = useSelector(state => state.notes)
```

## 3. แก้ไขส่วน render() ของ HomePage ให้ดึงค่าจาก this.props มาใช้

```jsx
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
    )
```

## A. ไฟล์เต็ม `pages/home-page/HomePage.js`

```jsx
import React, { Component } from 'react'
import { View } from 'react-native'
import { useSelector } from 'react-redux'
import { Container, Header, Title, Content, List, ListItem, Text, Left, Right, Body, Button, Icon } from 'native-base';


export default function HomePage() {

    const notes = useSelector(state => state.notes)

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
    )
}
```