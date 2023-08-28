
# 4. แสดงข้อมูล Users จาก Redux state ในหน้า HomePage

## 1. เข้าถึง redux state object ผ่าน react hook ชื่อ `useSelector`

โดยเราสามารถใช้ snippet ชื่อ useSelector ในการเขียนได้

```js
export default function HomePage() {

    const dispatch = useDispatch()
    const navigation = useNavigation()

    // useSelector จะได้รับ redux state object มา เราสามารถเลือก property ที่ต้องการเอามาเก็บในตัวแปร เพื่อใช้ใน component ได้
    const users = useSelector(state => state.users)
```

## 2. แสดงข้อมูล Users array ใน JSX

```js
    // ถ้ายังไม่มี users (ข้อมูลยังไม่พร้อมใช้จาก redux state) ให้แสดงเป็นคำว่า Loading
    if (users == undefined) {
        return <Text>Loading</Text>
    }

    return (
        <Container>
            <Content>
                <List>
                    {
                        users.map((item, index) => {
                            return (
                                <ListItem thumbnail
                                    button
                                    key={index}
                                    onPress={() => { openDetail(item) }}
                                >
                                    <Left>
                                        <Thumbnail source={{ uri: item.picture.thumbnail }} />
                                    </Left>
                                    <Body>
                                        <Text>{item.name.first} {item.name.last}</Text>
                                        <Text note>{item.phone}</Text>
                                    </Body>
                                    <Right></Right>
                                </ListItem>
                            )
                        })
                    }
                </List>
            </Content>
        </Container>
    )
```

## A. ไฟล์เต็ม 

```jsx
import React, { useEffect } from 'react'
import { Container, Content, List, ListItem, Text, Body, Left, Right, Thumbnail } from 'native-base';

import { useDispatch, useSelector } from 'react-redux';
import { useNavigation } from '@react-navigation/core';
import { createAction_UserSelected, startGetUser } from '../../redux/actions';

export default function HomePage() {

    const dispatch = useDispatch()
    const navigation = useNavigation()

    const users = useSelector(state => state.users)

    useEffect(() => {

        startGetUser(dispatch)

    }, [])

    const openDetail = (user) => {
        dispatch(createAction_UserSelected(user))
        navigation.navigate('Detail')
    }

    // ถ้ายังไม่มี users (ข้อมูลยังไม่พร้อมใช้จาก redux state) ให้แสดงเป็นคำว่า Loading
    if (users == undefined) {
        return <Text>Loading</Text>
    }

    return (
        <Container>
            <Content>
                <List>
                    {
                        users.map((item, index) => {
                            return (
                                <ListItem thumbnail
                                    button
                                    key={index}
                                    onPress={() => { openDetail(item) }}
                                >
                                    <Left>
                                        <Thumbnail source={{ uri: item.picture.thumbnail }} />
                                    </Left>
                                    <Body>
                                        <Text>{item.name.first} {item.name.last}</Text>
                                        <Text note>{item.phone}</Text>
                                    </Body>
                                    <Right></Right>
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