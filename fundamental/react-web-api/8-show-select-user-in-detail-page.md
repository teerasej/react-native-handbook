
# 8. แสดงรายละเอียด User ที่ถูกเลือกใน Detail Page

เปิดไฟล์ `pages/detail-page/DetailPage.js`

## 1. เริ่มจากกำหนดค่าใน mapStateToProps

```js
const mapStateToProps = (state) => ({
    selectedUser: state.app.selectedUser
})
```

## 2. แสดงข้อมูล User ที่ถูกเลือก ใน JSX 

```jsx
render() {

        const user = this.props.selectedUser;

        return (
            <Content padder>
                <List>
                    <ListItem>
                        <Text>ชื่อ: {user.name.first}</Text>
                    </ListItem>
                    <ListItem>
                        <Text>นามสกุล: {user.name.last}</Text>
                    </ListItem>
                </List>
                <Button iconLeft block 
                style={{
                    marginTop: 10
                }}>
                    <Icon name="call" />
                    <Text>โทร: {user.phone}</Text>
                </Button>
            </Content>
        )
    }
```

## A. ไฟล์เต็ม `pages/detail-page/DetailPage.js`

```jsx
import React, { Component } from 'react'
import { View } from 'react-native'
import { Content, List, ListItem, Text, Body, Button, Icon } from 'native-base';
import { connect } from 'react-redux'

import actions from "../../redux/actions";

export class DetailPage extends Component {

    static navigationOptions = {
        title: 'Detail'
    };

    render() {

        const user = this.props.selectedUser;

        return (
            <Content padder>
                <List>
                    <ListItem>
                        <Text>ชื่อ: {user.name.first}</Text>
                    </ListItem>
                    <ListItem>
                        <Text>นามสกุล: {user.name.last}</Text>
                    </ListItem>
                </List>
                <Button iconLeft block 
                style={{
                    marginTop: 10
                }}>
                    <Icon name="call" />
                    <Text>โทร: {user.phone}</Text>
                </Button>
            </Content>
        )
    }
}

const mapStateToProps = (state) => ({
    selectedUser: state.app.selectedUser
})

const mapDispatchToProps = {

}

export default connect(mapStateToProps, mapDispatchToProps)(DetailPage)
```