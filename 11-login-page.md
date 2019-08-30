# 11. สร้างและจัดการ Login Page 

## 1. สร้าง LoginPage 

```jsx
// pages/login-page/LoginPage.js

import React, { Component } from 'react'
import { View, Text } from 'react-native'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'

export class LoginPage extends Component {

    static navigationOptions = ({ navigation }) => {
        return {
            headerTitle: <Text>Login</Text>
        };
    };

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

export default connect(mapStateToProps, mapDispatchToProps)(LoginPage)
```

## 2. เชื่อม Login Page เข้ากับ Navigation

โดยเราสามารถกำหนดหน้าเริ่มต้นด้วย config property ที่ชื่อ `initialRouteName`

```js
// App.js

import LoginPage from './pages/login-page/LoginPage';

const AppNavigator = createStackNavigator({
    Home: { screen: HomePage },
    // เพิ่ม Login Page เข้าไปใน Navigator
    Login: { screen: LoginPage },
    CreateNote: { screen: NewNotePage }
    }, 
    
    // กำหนดค่าเริ่มต้นของ Page
    {
        initialRouteName: "Login"
    });
```

## 3. สร้าง Login Form 

```js
// pages/login-page/LoginForm.js

import React, { Component } from 'react'
import { View, Alert } from 'react-native'
import { Text, Button, Item, Input, Label } from 'native-base';
import { Field, reduxForm } from 'redux-form';

export class LoginForm extends Component {

    renderInput({ input, label, type, meta: { touched, error, warning } }) {

        console.log('rendering');
        console.log(input, type);

        var hasError = false;
        if (error !== undefined && touched) {
            hasError = true;
            Alert.alert(
                'Opps',
                error
            );
        }
        return (
                <Item error={hasError}>
                    { type === 'password' ?  <Input {...input} secureTextEntry={true}/> : <Input {...input}/>}
                </Item>
        )
    }

    render() {
        const { handleSubmit } = this.props;

        return (
            <View>
                <Item stackedLabel >
                    <Label>Username: </Label>
                    <Field name="username" component={this.renderInput} />
                </Item>
                <Item stackedLabel last>
                    <Label>Password: </Label>
                    <Field name="password" type="password" component={this.renderInput} />
                </Item>
                <Button block primary onPress={handleSubmit} >
                    <Text>Sign in</Text>
                </Button>
            </View>
        )
    }
}

const validate = values => {

    const error = {};
    error.username = '';
    error.password = '';

    let username = values.username;
    let password = values.password;

    if (values.username === undefined) {
        username = ''
    }
    if (values.password === undefined) {
        password = ''
    }

    if (password === '') {
        error.password = 'fill something';
    }
    if (password === '') {
        error.password = 'fill something';
    } 

    return error;
};

LoginForm = reduxForm({ form: 'login', validate })(LoginForm)

export default LoginForm

```

## 4. นำ Login Form มาใช้ใน Login Page

```jsx
// pages/login-page/LoginPage.js

import LoginForm from './LoginForm';
import { Content } from 'native-base'; 

onSignIn = (values) => {
    console.log(values);
}

render() {
    return (
        <Content padder>
            <LoginForm onSubmit={this.onSignIn} />
        </Content>
    )
}
```

## 4.A ไฟล์เต็ม `pages/login-page/LoginPage.js`

```jsx
import React, { Component } from 'react'
import { View, Text } from 'react-native'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'
import LoginForm from './LoginForm';
import { Content } from 'native-base';


export class LoginPage extends Component {

    static navigationOptions = ({ navigation }) => {
        return {
            headerTitle: <Text>Login</Text>
        };
    };

    onSignIn = (values) => {
        console.log(values);
    }

    render() {
        return (
            <Content padder>
                <LoginForm onSubmit={this.onSignIn} />
            </Content>
        )
    }
}

const mapStateToProps = (state) => ({

})

const mapDispatchToProps = {

}

export default connect(mapStateToProps, mapDispatchToProps)(LoginPage)
```