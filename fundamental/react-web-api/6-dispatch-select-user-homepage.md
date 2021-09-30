
# 6. Dispatch action หลังจากมีการเลือก User จากรายการ

เปิดไฟล์ `pages/home-page/HomePage.js`



## 1. เรียกใช้ function 

เรียกใช้ function `createAction_UserSelected` โดยการส่ง user object เข้าไป จาก function `openDetail()` ที่จะทำงานเมื่อมีการกดเลือกผู้ใช้

```js
const openDetail = (user) => {
    let action = createAction_UserSelected(user)
    dispatch(action)
    navigation.navigate('Detail')
}
```

## 3. ปรับ JSX ให้ส่ง object เข้าไปใน function เมื่อถูกกดเลือกจาก ListItem

```jsx
<List>
    {
        users.map((item, index) => {
            return (
                <ListItem thumbnail
                    button
                    key={index}
                    onPress={() => { openDetail(item) }}
                >
                
                ...
           
```


## A. ไฟล์เต็ม `pages/home-page/HomePage.js`

```js
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
        let action = createAction_UserSelected(user)
        dispatch(action)
        navigation.navigate('Detail')
    }

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