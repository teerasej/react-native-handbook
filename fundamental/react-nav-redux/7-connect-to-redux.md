
# 7. เชื่อม HomePage และ NewNotePage เข้า Redux

## 1. เชื่อม HomePage

เปิดไฟล์ `pages/home-page/HomePage.js`

เราจะสร้าง function ขึ้นมา 2 ตัว ซึ่ง 2 function นี้จะเป็นตัวเชื่อมต่อกับ redux 

และเราสามารถเชื่อมได้ผ่านการใช้งาน `connect()` function ที่ import มาจาก `react-redux` module

```js
// snippet: reduxmap
const mapStateToProps = (state) => ({

})

const mapDispatchToProps = {

}


// เปลี่ยนจาก export default HomePage เป็นด้านล่าง ซึ่งเรียกใช้ connect method
export default connect(mapStateToProps, mapDispatchToProps)(HomePage);
```

## 2. เชื่อม NewNotePage

เปิดไฟล์ `pages/new-note-page/NewNotePage.js`

```js
const mapStateToProps = (state) => ({

})

const mapDispatchToProps = {

}

export default connect(mapStateToProps, mapDispatchToProps)(NewNotePage)
```

## A. ไฟล์เต็ม `pages/home-page/HomePage.js`

```jsx
import React, { Component } from 'react'
import { View } from 'react-native'
import { connect } from 'react-redux'
import { Container, Header, Title, Content, List, ListItem, Text, Left, Right, Body, Button, Icon } from 'native-base';


export class HomePage extends Component {

    render() {
        return (

            <Content>
                <List>
                    <ListItem>
                        <Text>A</Text>
                    </ListItem>
                    <ListItem>
                        <Text>B</Text>
                    </ListItem>
                </List>
            </Content>

        )
    }
}

// snippet: reduxmap
const mapStateToProps = (state) => ({
    
})

const mapDispatchToProps = {
    
}


export default connect(mapStateToProps, mapDispatchToProps)(HomePage);
```

## B. ไฟล์เต็ม `pages/new-note-page/NewNotePage.js`

```jsx
import React, { Component } from 'react'
import { View } from 'react-native'
import { connect } from 'react-redux'
import { Container, Header, Title, Content, List, ListItem, Text, Left, Right, Body, Button, Item, Input, Label } from 'native-base';

import NewNoteForm from './NewNoteForm';

export class NewNotePage extends Component {
  
  static navigationOptions = {
    title: 'New Note'
  };

  onFormSave = (values) => {
    console.log(values);
      
    this.props.navigation.goBack();
  }


  render() {
    return (
      <Content padder>
        <NewNoteForm onSubmit={this.onFormSave}/>
      </Content>
    )
  }
}

const mapStateToProps = (state) => ({
  
})

const mapDispatchToProps = {
  
}


export default connect(mapStateToProps, mapDispatchToProps)(NewNotePage)
```