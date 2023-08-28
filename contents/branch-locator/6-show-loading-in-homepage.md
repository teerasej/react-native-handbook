
# 6. แสดงข้อมูลบนหน้า HomePage

## 1. แสดงข้อความ ถ้ายังไม่มี location จากอุปกรณ์

เปิดไฟล์ `pages/home-page/HomePage.js`

```js
render() {

        const { currentLocation } = this.props;


        if (!currentLocation) {
            return (
                <View style={{
                    flex: 1,
                    justifyContent: 'center',
                    alignItems: 'center'
                }}>
                    <Text>Locating you...</Text>
                </View>
            );
        }

        return (

            <MapView style={{flex: 1}} />

        )
    }
```

## 2. เริ่มขอตำแหน่งหลังจาก แสดงหน้า home page

```js
componentDidMount() {
    this.props.getCurrentLocation();
}

//...

const mapDispatchToProps = dispatch => {
    return {
        getCurrentLocation: () => actions.getCurrentLocation(dispatch)
    }
}

```

## 3. เพิ่มข้อมูลตำแหน่งของอุปกรณ์เข้าไปใน app state

เปิดไฟล์ `redux/reducers/app.reducer.js`

```js
export default (state = initialState, { type, payload }) => {
    switch (type) {

    case actions.Types.LOCATION_DEVICE_FOUND: {
        return { ...state, currentLocation: payload }
    }

    default:
        return state
    }

```

## 4. แสดงตำแหน่งของเครื่องในแผนที่

โดยการดึงข้อมูลจาก props ที่ได้จาก app state

เปิดไฟล์ `pages/home-page/HomePage.js`

```js
return (

            <MapView
                style={{ flex: 1 }}
                ref={map => { this.map = map }}
                initialRegion={{
                    latitude: this.props.currentLocation.coords.latitude,
                    longitude: this.props.currentLocation.coords.longitude,
                    latitudeDelta: 0.0922,
                    longitudeDelta: 0.0421,
                }}
            />

)
// ...
const mapStateToProps = (state) => ({
    currentLocation: state.app.currentLocation
})
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
        this.props.getCurrentLocation();
    }

    render() {

        const { currentLocation } = this.props;

        if (!currentLocation) {
            return (
                <View style={{
                    flex: 1,
                    justifyContent: 'center',
                    alignItems: 'center'
                }}>
                    <Text>Locating you...</Text>
                </View>
            );
        }

        return (

            <MapView
                style={{ flex: 1 }}
                ref={map => { this.map = map }}
                initialRegion={{
                    latitude: this.props.currentLocation.coords.latitude,
                    longitude: this.props.currentLocation.coords.longitude,
                    latitudeDelta: 0.0922,
                    longitudeDelta: 0.0421,
                }}
            />

        )
    }
}

const mapStateToProps = (state) => ({
    currentLocation: state.app.currentLocation
})

const mapDispatchToProps = dispatch => {
    return {
        getCurrentLocation: () => actions.getCurrentLocation(dispatch)
    }
}

export default connect(mapStateToProps, mapDispatchToProps)(HomePage)
```