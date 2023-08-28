
# 2. เรียกใช้ Action function ใน HomePage 

เปิดไฟล์ `pages/home-page/HomePage.js`

## 1. เรียกใช้ dispatch function ของ redux ผ่าน react hook ชื่อ `useDispatch()`

```js
export default function HomePage() {

    const dispatch = useDispatch()

    //...

}
```

## 2. เรียกใช้ function ตอนหน้า HomePage แสดงขึ้นมาแล้ว 

- ในที่นี้เราจะใช้ react hook ที่ชื่อ `useEffect` ในการรันคำสั่งหลังจาก render component ขึ้นมาแล้ว
- function `startGetUser` เราสร้างเอาไว้ใน `redux/action.js`
- และเราต้องการ dispatch action จากภายใน function เราเลยส่ง dispatch เข้าไปเพื่อใช้งาน

```js
useEffect(() => {
    startGetUser(dispatch)
}, [])
```

## A. ไฟล์เต็ม `pages/home-page/HomePage.js`

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