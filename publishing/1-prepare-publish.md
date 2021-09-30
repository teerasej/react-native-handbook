
# 1. การเตรียมโปรเจค

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
      "bundleIdentifier": "th.in.nextflow.react.native.noteapp",
      "buildNumber": "1.0.0"
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
- **buildNumber** เวอร์ชั่นที่จะกำหนดลงไปในแอพ 

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
- **versionCode** เป็นเลขเวอร์ชั่นที่กำหนดให้กับแต่ละ release ตัว Play Store จะใช้เลขนี้แยกแยะความใหม่เก่าของไฟล์ที่อัพโหลดเข้า Play Store ดังนั้นควรเปลี่ยนให้เพิ่มขึ้นในแต่ละครั้งที่จะ release ขึ้น Play Store
- ค่าในตัวอย่างเป็นพื้นฐานที่จำเป็นต้องใช้สำหรับการส่งแอพขึ้น Play Store 

```json
"android": {
      "package": "th.in.nextflow.react.native.noteapp",
      "versionCode": 1,
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

### icon.png

![icon](https://user-images.githubusercontent.com/85179/113394518-99e82b00-93c2-11eb-9193-c091d6ecfba6.png)

## splash.png

![splash](https://user-images.githubusercontent.com/85179/113394765-06632a00-93c3-11eb-9106-fac46fcf8b37.png)

