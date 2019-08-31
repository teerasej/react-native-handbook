
# 1. ตั้งค่าโปรเจค

## 1. กำหนดค่าเบื้องต้นใน `app.json`

เปิดไฟล์ `app.json` 

```json
{
  "expo": {
    "name": "note-mobile-app",
    "slug": "nextflow-note-mobile-app",
    "privacy": "public",
    "sdkVersion": "34.0.0",
    "platforms": [
      "ios",
      "android",
      "web"
    ],
    "version": "1.0.1",
    "orientation": "portrait",
    "icon": "./assets/icon.png",
    "splash": {
      "image": "./assets/splash.png",
      "resizeMode": "contain",
      "backgroundColor": "#ffffff"
    },
    "updates": {
      "fallbackToCacheTimeout": 0
    },
    "assetBundlePatterns": [
      "**/*"
    ],
    "ios": {
      "supportsTablet": true,
      "bundleIdentifier": "th.in.nextflow.react.native.noteapp"
    },
    "androidStatusBar": {
      "barStyle": "light-content",
      "backgroundColor": "#C2185B"
    }
  }
}
```

### ค่าที่ต้องเพิ่มเข้าไป สำหรับ iOS 

> [ดูอ้างอิงการกำหนดค่า iOS](https://docs.expo.io/versions/latest/workflow/configuration/#ios)

- **bundleIdentifier** เป็นค่า App ID ที่จะใช้กำหนดกับแอพ iOS จะเขียนเป็นแบบ reverse domain name

```json
"ios": {
      //...
      "bundleIdentifier": "th.in.nextflow.react.native.noteapp"
}
```


### ค่าที่ต้องเพิ่มเข้าไป สำหรับ Android  

> [ดูอ้างอิงการกำหนดค่า Android](https://docs.expo.io/versions/latest/workflow/configuration/#android)

- **package** เป็นค่า App ID ที่จะใช้กำหนดกับแอพ Android จะเขียนเป็นแบบ reverse domain name
- **permissions** เป็นการกำหนด android permission แบบเดียวกับใน `AndroidManifest.xml` 
- ค่าในตัวอย่างเป็นพื้นฐานที่จำเป็นต้องใช้สำหรับการส่งแอพขึ้น Play Store 

```json
"android": {
      "package": "th.in.nextflow.react.native.noteapp",
      "versionCode": 2,
      "permissions": [
        "CAMERA",
        "READ_CONTACTS",
        "READ_PHONE_STATE",
        "RECORD_AUDIO"
      ]
    },
```


## 2. กำหนดไฟล์ App Icon และ Splash 

ไฟล์ App Icon และ Splash Screen จะอยู่ในโฟลเดอร์​ `/assets` ของโปรเจค

- assets/icon.png 
    - ควรมีขนาด 1024 x 1024 px หรือ 192 x 192 px
    - เป็นไฟล์ png
- assets/splash.png 
    - ควรมีขนาด 1242 x 2436 px 
    - เป็นไฟล์ png 