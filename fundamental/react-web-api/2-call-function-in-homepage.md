
# 2. เรียกใช้ Action function ใน HomePage 

เปิดไฟล์ `pages/home-page/HomePage.js`

## 1. กำหนด function ให้ props ผ่าน mapDispatchToProps

```js
const mapDispatchToProps = dispatch => ({
    startGetUser: () => actions.startGetUser(dispatch)
})
```

## 2. เรียกใช้ function ตอนหน้า HomePage แสดงขึ้นมาแล้ว 

```js
componentDidMount() {
        this.props.startGetUser();
}
```

## A. ไฟล์เต็ม `pages/home-page/HomePage.js`

```jsx
mport React, { Component } from 'react'
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
        return (
            <Content padder>
                <List>
                    <ListItem button onPress={() => this.openDetail()}>
                        <Text>Hello</Text>
                    </ListItem>
                </List>
            </Content>
        )
    }
}

const mapStateToProps = (state) => ({
    
})

const mapDispatchToProps = dispatch => ({
    startGetUser: () => actions.startGetUser(dispatch)
})

export default connect(mapStateToProps, mapDispatchToProps)(HomePage)
```