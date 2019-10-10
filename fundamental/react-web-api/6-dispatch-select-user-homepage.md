
# 6. Dispatch action หลังจากมีการเลือก User จากรายการ

เปิดไฟล์ `pages/home-page/HomePage.js`

## 1. เริ่มจากกำหนด function ผ่าน mapDispatchToProps

```js
const mapDispatchToProps = dispatch => ({
    startGetUser: () => actions.startGetUser(dispatch),
    selectUser: (user) => actions.selectUser(user)
})
```

## 2. เรียกใช้ function 

```js
openDetail = (user) => {
    this.props.selectUser(user);
    this.props.navigation.navigate('Detail');
}
```

## 3. ปรับ JSX ให้ส่ง object เข้าไปใน function เมื่อถูกกดเลือกจาก ListItem

```jsx
<List>
    {
        this.props.users.map((item, index) => {
            return (
                <ListItem thumbnail
                    button
                    key={index}
                    onPress={() => this.props.selectUser(item)}
                >
                    ...
           
```


## A. ไฟล์เต็ม `pages/home-page/HomePage.js`

```js
import React, { Component } from 'react'
import { View } from 'react-native'
import { connect } from 'react-redux'
import { Content, List, ListItem, Text, Body, Button, Icon, Left, Right, Thumbnail } from 'native-base';
import actions from "../../redux/actions";

export class HomePage extends Component {

    static navigationOptions = {
        title: 'Contacts'
    };

    componentDidMount() {
        this.props.startGetUser();
    }

    openDetail = (user) => {
        this.props.selectUser(user);
        this.props.navigation.navigate('Detail');
    }

    render() {

        if(this.props.users == undefined){
            return <Text></Text>
        }

        return (
            <Content>
                <List>
                    {
                        this.props.users.map((item, index) => {
                            return (
                                <ListItem thumbnail
                                    button
                                    key={index}
                                    onPress={() => this.openDetail(item)}
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
        )
    }
}

const mapStateToProps = (state) => ({
    users: state.app.users
})

const mapDispatchToProps = dispatch => ({
    startGetUser: () => actions.startGetUser(dispatch),
    selectUser: (user) => dispatch(actions.selectUser(user))
})

export default connect(mapStateToProps, mapDispatchToProps)(HomePage)
```