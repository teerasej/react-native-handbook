
# 4. แสดงข้อมูล Users จาก Redux state ในหน้า HomePage

## 1. ทำการ Map ค่าที่ได้จาก state เข้า props object

```js
const mapStateToProps = (state) => ({
    users: state.app.users
})
```

## 2. แสดงข้อมูล Users array ใน JSX

```js
render() {

        // ถ้ายังไม่มี props.user (ข้อมูลยังไม่พร้อมใช้จาก redux state) ให้แสดงเป็นคำว่า Loading
        if(this.props.users == undefined){
            return <Text>Loading...</Text>
        }

        return (
            <Content>
                <List>
                    {
                        this.props.users.map((item, index) => {
                            return (
                                <ListItem thumbnail>
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
```

## A. ไฟล์เต็ม `

```jsx
import React, { Component } from 'react'
import { View } from 'react-native'
import { connect } from 'react-redux'
import { Content, List, ListItem, Text, Body, Button, Icon, Left, Thumbnail, Right } from 'native-base';
import actions from "../../redux/actions";

export class HomePage extends Component {

    static navigationOptions = {
        title: 'Contacts'
    };

    componentDidMount() {
        this.props.startGetUser();
    }

    openDetail = () => {
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
                                <ListItem thumbnail>
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
    startGetUser: () => actions.startGetUser(dispatch)
})

export default connect(mapStateToProps, mapDispatchToProps)(HomePage)
```