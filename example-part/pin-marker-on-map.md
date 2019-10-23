
# Pin Marker on Map

> ดู Document เพิ่มเติมได้ที่ [Expo Google Map](https://docs.expo.io/versions/latest/sdk/map-view/)

- [Download Starter project](https://www.dropbox.com/s/0p6o6e0qghtcxyn/react-native-branch-map-mapview-starter.zip?dl=0)
- [Download finish project](https://www.dropbox.com/s/ok1094ayq41ytya/react-native-branch-map-finish.zip?dl=0)


## 1. แสดง MapView บนหน้า Home Page

เปิดไฟล์ `pages/home-page/HomePage.js`

ใช้งาน `MapView` component แทน ใน `render()`

```js
return (
    <MapView
        style={{ flex: 1 }}
        ref={map => { this.map = map }}
        provider={MapView.PROVIDER_GOOGLE}
    >
    </MapView>
)
```

## 2. กำหนดตำแหน่งเริ่มต้นให้แผนที่ 

ผ่าน props `initialRegion` ของ `MapView`

ในที่นี่เราดึงตำแหน่งมาจาก Redux ในชื่อของ `currentLocation`

```js
return (
    <MapView
        style={{ flex: 1 }}
        ref={map => { this.map = map }}
        provider={MapView.PROVIDER_GOOGLE}
        initialRegion={{
                    latitude: this.props.currentLocation.coords.latitude,
                    longitude: this.props.currentLocation.coords.longitude,
                    latitudeDelta: 0.0922,
                    longitudeDelta: 0.0421,
        }}
    >
    </MapView>
)
```

## 3. นำข้อมูลตำแหน่งสาขาที่มีจาก Redux มา Plot เป็น Marker


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
```