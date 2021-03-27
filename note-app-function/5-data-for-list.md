
# 5. กำหนดค่าเพื่อใช้แสดงผลใน List

## 1. กำหนดค่า PropTypes และ defaultProps และข้อมูลเพื่อใช้งาน

- [PropTypes](https://reactjs.org/docs/typechecking-with-proptypes.html)
- [Default Props](https://blog.logrocket.com/a-complete-guide-to-default-props-in-react-984ea8e6972d/)

เตรียมข้อมูลสำหรับ List ก่อน แสดงผลลัพธ์

```js
// pages/home-page/HomePage.js

export default function HomePage() {

    let notes = [
        { title: 'a' },
        { title: 'b' },
        { title: 'c' }
    ]

    // ...
```

## 2. ปรับส่วน Render ให้แสดงข้อมูลจาก props

```jsx
// pages/home-page/HomePage.js
export default function HomePage() {

    let notes = [
        { title: 'a' },
        { title: 'b' },
        { title: 'c' }
    ]

    return (
        <Container>
            <Header>
                <Body>
                    <Title>Note</Title>
                </Body>
            </Header>
            <Content>
                <List>
                    {
                        // .map() function จะทำการวนลูปกับ item ใน array ทุกตัว
                        // โดยทำการส่ง item เข้าในใน function พร้อมเลข index ทีละตัว
                        // ในที่นี้เราประยุกต์โดยการส่ง jsx กลับออกไป
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
        </Container>
    )
}

```

## ไฟล์เต็ม `pages/home-page/HomePage.js`

```jsx
import { Body, Container, Content, Header, List, ListItem, Title } from 'native-base'
import React from 'react'
import { View, Text } from 'react-native'

export default function HomePage() {

    let notes = [
        { title: 'a' },
        { title: 'b' },
        { title: 'c' }
    ]

    return (
        <Container>
            <Header>
                <Body>
                    <Title>Note</Title>
                </Body>
            </Header>
            <Content>
                <List>
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
        </Container>
    )
}

```
