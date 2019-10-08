
# Event ใน Class component

สำหรับ Class component เราสามารถำหนด function ให้กับ event ได้ผ่าน keyword `this`

```jsx
import React, { Component } from 'react';
import { StyleSheet, Text, View, Button, Alert } from 'react-native';
import Hello from "./Hello";
import Goodbye from "./Goodbye"; 

export default class App extends Component {

  popUpAlert = () => {
    Alert.alert('oops...', 'you do something');
  }

  render() {
    let username = 'พล';
    let website = 'nextflow.in.th'

    return (
      <View style={styles.container}>
        <Text>Open up App.js to start working on your app!</Text>
        <Hello name="พล" website="nextflow.in.th" />
        <Goodbye name={username} website={website} />
        <Button title="ลงชื่อเข้าใช้" color={styles.signInButton.color}
          onPress={this.popUpAlert}
        />
      </View>
    );
  }

}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
  helloText: {
    fontSize: 30,
    color: 'green'
  },
  signInButton: {
    color: 'orange'
  }
});

```