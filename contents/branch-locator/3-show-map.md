
# 3. แสดงแผนที่บนหน้า Home 

> [Expo Map View](https://docs.expo.io/versions/latest/sdk/map-view/)

## 1. ติดตั้ง Google Map Plugin 

```bash
expo install react-native-maps
```

## 2. เพิ่ม Google Map API Key ลงใน app.json 

เปิดไฟล์ `app.json`

### สำหรับ Android 

```json
{
  "expo": {
    ...
    "android": {
      ...
      "config": {
        "googleMaps": {
          "apiKey": "ใส่ api key ตรงนี้"
        }
      }
    }
  }
}
```

### สำหรับ iOS 

```json
{
  "expo": {
    ...
    "ios": {
      ...
      "config": {
        "googleMapsApiKey": "AIzaSyDSHV3gPFfzDc0fgt9vLuCULcz3Qfcp--Q"
      }
    }
  }
}
```

## 3. แสดงแผนที่

เปิดไฟล์ `pages/home-page/HomePage.js`

```js
import MapView from 'react-native-maps'

render() {
        return (
            <MapView style={{flex: 1}} />
        )
}

```


## A. ไฟล์เต็ม `pages/home-page/HomePage.js`

```js
import React, { Component } from 'react'
import { View } from 'react-native'
import { Content, List, ListItem, Text, Body, Button, Icon } from 'native-base';
import { connect } from 'react-redux'
import actions from '../../redux/actions';
import MapView from 'react-native-maps'

export class HomePage extends Component {
    static navigationOptions = ({ navigation }) => {
        return {
            title: 'Home'
        }
    };

    componentDidMount() {
       
    }

    render() {
        return (
            <MapView style={{flex: 1}} />
        )
    }
}

const mapStateToProps = (state) => ({
})

const mapDispatchToProps = dispatch => {
    return {
        
    }
}

export default connect(mapStateToProps, mapDispatchToProps)(HomePage)
```