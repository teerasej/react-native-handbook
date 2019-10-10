
# 11. เรียกใช้ action function ผ่าน Detail Page

เปิดไฟล์ `pages/detail-page/DetailPage.js`

## 1. สร้าง function ใน mapStateToProps 

```js
const mapDispatchToProps = dispatch => {
    return {
        call: (phoneNumber) => actions.makeCall(phoneNumber)
    }
}
```

## 2. เพิ่ม onPress event 

```jsx
<Button iconLeft block 
    onPress={() => this.props.call(user.phone)}
    style={{
        marginTop: 10
    }}>
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
                onPress={() => this.props.call(user.phone)}
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

const mapDispatchToProps = dispatch => {
    return {
        call: (phoneNumber) => actions.makeCall(phoneNumber)
    }
}

export default connect(mapStateToProps, mapDispatchToProps)(DetailPage)
```