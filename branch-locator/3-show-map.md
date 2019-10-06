
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