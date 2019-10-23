
# 7. สร้างหน้า New Note Page

## 1. สร้างไฟล์ NewNotePage.js

```jsx
// pages/new-note-page/NewNotePage.js

import React, { Component } from 'react'
import { View, Text } from 'react-native'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'

export class NewNotePage extends Component {
    static propTypes = {
        prop: PropTypes
    }

    render() {
        return (
            <View>
                <Text> prop </Text>
            </View>
        )
    }
}

const mapStateToProps = (state) => ({

})

const mapDispatchToProps = {

}

export default connect(mapStateToProps, mapDispatchToProps)(NewNotePage)
```

## 2. สร้างส่วนของ UI ด้วย Nativebase

```jsx
import React, { Component } from 'react'
import { View } from 'react-native'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'
import { Container, Header, Title, Content, List, ListItem, Text, Left, Right, Body, Button } from 'native-base';


export class NewNotePage extends Component {
    static propTypes = {
       
    }

    render() {
        return (
            <Container>
                <Header>
                    <Body>
                        <Title>New Note</Title>
                    </Body>
                </Header>
                <Content padder>
                    <Button block primary >
                        <Text>Save</Text>
                    </Button>
                </Content>
            </Container>
        )
    }
}

```

## 3. แยกส่วน Form ไปไว้ใน Component ของตัวเอง เพื่อใช้กับ redux-form

```jsx
// pages/new-note-page/NewNoteForm.js

import React, { Component } from 'react'
import { View } from 'react-native'
import {  Text, Button, Item, Input, Label } from 'native-base';
import { Field, reduxForm } from 'redux-form';

export class NewNoteForm extends Component {

    // ส่วนนี้ใช้ในการสร้าง Component ที่เป็น Input ส่งให้กับ Field Component
    renderInput({ input, label, type, meta: { touched, error, warning } }) {
        var hasError = false;
        if (error !== undefined) {
            hasError = true;
        }
        return (
            <Item error={hasError}>
                <Input {...input} />
                {hasError ? <Text>{error}</Text> : <Text />}
            </Item>
        )
    }


    render() {

        // ดึงมาจาก reduxForm ที่ HOC กับ NewNoteForm
        const { handleSubmit } = this.props;

        return (
            <View>
                <Item stackedLabel>
                    <Label>Message: </Label>
                    <Field name="message" component={this.renderInput} />
                </Item>
                <Button block primary onPress={handleSubmit} >
                    <Text>Save</Text>
                </Button>
            </View>
        )
    }
}

NewNoteForm = reduxForm({ form: 'newNote' })(NewNoteForm)

export default NewNoteForm
```

## 4. ปรับหน้า NewNotePage ให้ใช้ NewNoteForm 

```jsx
import React, { Component } from 'react'
import { View } from 'react-native'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'
import { Container, Header, Title, Content, List, ListItem, Text, Left, Right, Body, Button, Item, Input, Label } from 'native-base';

import NewNoteForm from './NewNoteForm';

export class NewNotePage extends Component {
  static propTypes = {

  }

  static navigationOptions = {
    title: 'New Note'
  };

  onFormSave = (values) => {
    console.log(values);
  }


  render() {
    return (
            <Container>
                <Header>
                    <Body>
                        <Title>New Note</Title>
                    </Body>
                </Header>
                <Content padder>
                    <NewNoteForm onSubmit={this.onFormSave}/>
                </Content>
            </Container>
      
    )
  }
}

```
## 4.A ไฟล์เต็ม `pages/new-note-page/NewNotePage.js`

```jsx
import React, { Component } from 'react'
import { View } from 'react-native'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'
import { Container, Header, Title, Content, List, ListItem, Text, Left, Right, Body, Button, Item, Input, Label } from 'native-base';

import NewNoteForm from './NewNoteForm';

export class NewNotePage extends Component {
  static propTypes = {

  }

  static navigationOptions = {
    title: 'New Note'
  };

  onFormSave = (values) => {
    console.log(values);
  }


  render() {
    return (
            <Container>
                <Header>
                    <Body>
                        <Title>New Note</Title>
                    </Body>
                </Header>
                <Content padder>
                    <NewNoteForm onSubmit={this.onFormSave}/>
                </Content>
            </Container>
      
    )
  }
}

const mapStateToProps = (state) => ({

})

const mapDispatchToProps = {

}



export default connect(mapStateToProps, mapDispatchToProps)(NewNotePage)

```

## 5. แสดง NewNotePage ในหน้าแอพ

```jsx
// App.js

import  NewNotePage  from './pages/new-note-page/NewNotePage';

render() {
    return (
      
      <NewNotePage />
    
    )
}
```
