
# 1. Setup Environment

ส่วนนี้จะเริ่มทำได้ ต้องมีการติดตั้งโปรแกรมให้ตรงกับระบบที่ใช้นะครับ
- [ใช้ Mac ทำแอพ iOS](https://nextflow.in.th/2017/setup-mac-os-ios-react-native/)
- [ใช้ Mac ทำแอพ Android](https://nextflow.in.th/2017/setup-react-native-on-macos-for-android-app-development/)
- [ใช้ Windows](https://nextflow.in.th/2017/install-react-native-for-window-for-android-app-dev/)

## 1. ติดตั้ง Expo CLI และ Package ที่เกี่ยวข้อง

> บางคนที่ใช้ Mac OS อาจต้องใช้ `sudo` นำหน้านะ 

- ถ้าใช้ **Windows** เปิดโปรแกรม Command Prompt หรือ Powershell
- ถ้าใช้ **MacOS** เปิดโปรแกรม Terminal

```bash
npm i -g expo-cli
npm i -g exp
npm i -g create-react-native
npm i -g react-native-cli
```

## 2. สร้างโปรเจค 

```bash
expo init nextflow-note-app
```

## 3. ติดตั้ง Module 

```bash
npm install redux react-redux redux-saga redux-form react-navigation react-navigation-stack redux-logger
```

หรือ

```bash
yarn add redux react-redux redux-saga redux-form react-navigation react-navigation-stack redux-logger
```




