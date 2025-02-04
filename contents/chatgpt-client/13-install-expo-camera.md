
# ติดตั้ง Expo Camera Plugin

## 1. ติดตั้ง Expo Camera Plugin

รันคำสั่ง ด้านล่างเพื่อติดตั้ง Expo Camera Plugin ในโปรเจค

```
expo install expo-camera 
```
หรือถ้าไม่ได้ ให้ลอง

```
npx expo install expo-camera    
```

## 2. เพิ่ม Config ใน `app.json`

เพิ่ม config ใน `app.json` เพื่อ[กำหนด Permission ตามคู่มือ](https://docs.expo.dev/versions/v51.0.0/sdk/camera/#example-appjson-with-config-plugin)ดังนี้

> สังเกตว่า มี **expo > plugins > expo-camera**

```json
{
  "expo": {
    "name": "test-expo",
    "slug": "test-expo",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/images/icon.png",
    "scheme": "myapp",
    "userInterfaceStyle": "automatic",
    "newArchEnabled": true,
    "ios": {
      "supportsTablet": true
    },
    "android": {
      "adaptiveIcon": {
        "foregroundImage": "./assets/images/adaptive-icon.png",
        "backgroundColor": "#ffffff"
      }
    },
    "web": {
      "bundler": "metro",
      "output": "static",
      "favicon": "./assets/images/favicon.png"
    },
    "plugins": [
      "expo-router",
      [
        "expo-splash-screen",
        {
          "image": "./assets/images/splash-icon.png",
          "imageWidth": 200,
          "resizeMode": "contain",
          "backgroundColor": "#ffffff"
        }
      ],
      [
        "expo-camera",
        {
          "cameraPermission": "Allow $(PRODUCT_NAME) to access your camera",
          "microphonePermission": "Allow $(PRODUCT_NAME) to access your microphone",
          "recordAudioAndroid": true
        }
      ]
    ],
    "experiments": {
      "typedRoutes": true
    }
  }
}
```

บันทึกไฟล์ให้เรียบร้อย