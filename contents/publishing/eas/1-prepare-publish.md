
# การเตรียมโปรเจค

สมัคร Expo Dev Account https://expo.dev/signup

## 1. ติดตั้ง eas-cli 

รันคำสั่งใน terminal หรือ command prompt

```bash
# Windows
npm install -g eas-cli

# MacOS
sudo npm install -g eas-cli
```

## 2. Login ด้วย Expo Account

รันคำสั่งด้านล่างเพื่อ login ด้วย Expo Account

```bash
eas login
```

## 3. รันคำสั่งสร้่างไฟล์ config สำหรับ build

รันคำสั่งด้านล่าง ภายใน root directory ของโปรเจค

```
eas build:configure
```

จะได้ไฟล์ `eas.json` ออกมา ซึ่งจะใช้สำหรับการ build แอพ ประมาณด้านล่าง

```json
{
  "cli": {
    "version": ">= 15.0.2",
    "appVersionSource": "remote"
  },
  "build": {
    "development": {
      "developmentClient": true,
      "distribution": "internal"
    },
    "preview": {
      "distribution": "internal",
      "android": {
        "buildType": "apk"
      }
    },
    "production": {
      "autoIncrement": true
    }
  },
  "submit": {
    "production": {}
  }
}
```

## 4. ทดสอบ Build APK ของแอพ Android 

ให้แน่ใจว่าได้เพิ่ม config ของ **build:preview** ในไฟล์ `eas.json` แล้ว

```json
{
  "build": {
    "preview": {
      "distribution": "internal",
      "android": {
        "buildType": "apk"
      }
    }
  }
}
```

บันทึกไฟล์ 

## 5. ตรวจสอบการตั้งค่า

ให้แน่ใจว่าในไฟล์ `app.json` ได้มีการเปลี่ยนชื่อ splash screen จาก

```json
"expo-splash-screen",
{
  "image": "./assets/images/splash-icon.png",
  "imageWidth": 200,
  "resizeMode": "contain",
  "backgroundColor": "#ffffff"
},
```
เป็น

```json
"expo-splash-screen",
{
  "image": "./assets/images/splash.png",
  "imageWidth": 200,
  "resizeMode": "contain",
  "backgroundColor": "#ffffff"
},
```

## 6.กำหนดไฟล์ App Icon และ Splash 

ไฟล์ App Icon และ Splash Screen จะอยู่ในโฟลเดอร์​ `/assets` ของโปรเจค

- assets/images/icon.png (iOS)
    - ควรมีขนาด 1024 x 1024 px 
    - เป็นไฟล์ png
- assets/images/adaptive-icon.png (Android)
    - ควรมีขนาด 1024 x 1024 px 
    - เป็นไฟล์ png
- assets/image/splash.png 
    - ควรมีขนาดเท่ากับภาพตัวอย่าง 
    - เป็นไฟล์ png 

> สามารถปรับเปลี่ยนที่อยู่ของไฟล์รูป App Icon และ Splash Screen ได้ในไฟล์ `app.json`

### icon.png

![icon](https://user-images.githubusercontent.com/85179/113394518-99e82b00-93c2-11eb-9193-c091d6ecfba6.png)

### splash.png

![splash](https://user-images.githubusercontent.com/85179/113394765-06632a00-93c3-11eb-9106-fac46fcf8b37.png)



## 6. รันคำสั่ง build

รันสั่งคำสั่งด้านล่าง เพื่อเริ่มกระบวนการ build

```bash
eas build -p android --profile preview
```

ระบบจะทำการสร้าง build บน EAS Cloud และเมื่อการ build สมบูรณ์จะส่งไฟล์ APK เพื่อให้เรามาใช้ติดตั้งบนอุปกรณ์ได้ 




## ภาคผนวก

- [Expo: status bar, app icon, and splash screen](https://docs.expo.dev/tutorial/configuration/)
- การสร้างไฟล์ APK สำหรับ Android หรือไฟล์ IPA สำหรับ iOS เพื่ออัพขึ้น App Store ด้วย EAS สามารถอ่านเพิ่มเติมได้ที่ [https://docs.expo.dev/build/setup/#run-a-build](https://docs.expo.dev/build/setup/#run-a-build)