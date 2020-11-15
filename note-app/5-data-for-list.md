
# 5. กำหนดค่าเพื่อใช้แสดงผลใน List

## 1. กำหนดค่า PropTypes และ defaultProps และข้อมูลเพื่อใช้งาน

- [PropTypes](https://reactjs.org/docs/typechecking-with-proptypes.html)
- [Default Props](https://blog.logrocket.com/a-complete-guide-to-default-props-in-react-984ea8e6972d/)

เตรียมข้อมูลสำหรับ List ก่อน แสดงผลลัพธ์

```js
// pages/home-page/HomePage.js

export class HomePage extends Component {
    static propTypes = {

        notes: PropTypes.array
    }

    static defaultProps = {
        notes: [
            { title: 'a' },
            { title: 'b' },
            { title: 'c' }
        ]
    }

```

## 2. ปรับส่วน Render ให้แสดงข้อมูลจาก props

```jsx
// pages/home-page/HomePage.js
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
```

## ไฟล์เต็ม `pages/home-page/HomePage.js`

```jsx
import React, { Component } from 'react'
import { View } from 'react-native'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'
import { Container, Header, Title, Content, List, ListItem, Text,  Body, } from 'native-base';

export class HomePage extends Component {
    static propTypes = {
        notes: PropTypes.array
    }

    static defaultProps = {
        notes: [
            { title: 'a' },
            { title: 'b' },
            { title: 'c' }
        ]
    }


    render() {
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
            </Container>

        )
    }
}

const mapStateToProps = (state) => ({

})

const mapDispatchToProps = {

}

export default connect(mapStateToProps, mapDispatchToProps)(HomePage)
```
