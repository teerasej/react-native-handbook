
# 10. ปักหมุดสาขาในแผนที่

เปิดไฟล์ `pages/home-page/HomePage.js`

```js
import MapView, { Marker } from 'react-native-maps'

export class HomePage extends Component {

    //...

    componentDidMount() {
        this.props.getCurrentLocation();
        this.props.getBranchesData();
    }

    render() {

        const { currentLocation, branches } = this.props;

        //...

        if (!branches) {
            return (
                <View style={{
                    flex: 1,
                    justifyContent: 'center',
                    alignItems: 'center'
                }}>
                    <Text>Getting branches...</Text>
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
                provider={MapView.PROVIDER_GOOGLE}
            >
                {branches.map(branch => (
                    <Marker
                        key={branch.id}
                        coordinate={{
                            latitude: branch.position.lat,
                            longitude: branch.position.lng
                        }}
                        title={branch.name}
                        description={branch.name}
                    />
                ))}
            </MapView>

        )
    }
}

const mapStateToProps = (state) => ({
    currentLocation: state.app.currentLocation,
    // รับข้อมูลสาขาจาก App state
    branches: state.app.branches
})

const mapDispatchToProps = dispatch => {
    return {
        getCurrentLocation: () => actions.getCurrentLocation(dispatch),

        // สร้าง function ที่เริ่มขอข้อมูล
        getBranchesData: () => actions.getBranchLocation(dispatch)
    }
}
```

## ไฟล์เต็ม `pages/home-page/HomePage.js`

```js
import React, { Component } from 'react'
import { View } from 'react-native'
import { Content, List, ListItem, Text, Body, Button, Icon } from 'native-base';
import { connect } from 'react-redux'
import actions from '../../redux/actions';
import MapView, { Marker } from 'react-native-maps'

export class HomePage extends Component {


    static navigationOptions = ({ navigation }) => {
        return {
            title: 'Home',
            // ปิดปุ่มทางด้านซ้าย
            headerLeft: null
        };
    };

    componentDidMount() {
        this.props.getCurrentLocation();
        this.props.getBranchesData();
    }
    

    render() {

        const { currentLocation, branches } = this.props;


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

        if (!branches) {
            return (
                <View style={{
                    flex: 1,
                    justifyContent: 'center',
                    alignItems: 'center'
                }}>
                    <Text>Getting branches...</Text>
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
                provider={MapView.PROVIDER_GOOGLE}
            >
                {branches.map(branch => (
                    <Marker
                        key={branch.id}
                        coordinate={{
                            latitude: branch.position.lat,
                            longitude: branch.position.lng
                        }}
                        title={branch.name}
                        description={branch.name}
                    />
                ))}
            </MapView>

        )
    }
}

const mapStateToProps = (state) => ({
    currentLocation: state.app.currentLocation,
    branches: state.app.branches
})

const mapDispatchToProps = dispatch => {
    return {
        getCurrentLocation: () => actions.getCurrentLocation(dispatch),
        getBranchesData: () => actions.getBranchLocation(dispatch)
    }
}

export default connect(mapStateToProps, mapDispatchToProps)(HomePage)
```