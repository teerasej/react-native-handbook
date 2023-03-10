
# Setup Project

# NPM WARNING!!!

npm เวอร์ชั่น 7 มีการเปลี่ยนแปลงการทำงานจาก npm เวอร์ชั่น 6 และพังการทำงานของหลายๆ package รวมถึง package ที่ใช้ใน React Native และ Expo ด้วย 

จึงแนะนำให้ใช้ npm 6 แทน โดยใช้คำสั่งด้านล่าง

```bash
# เช็คเวอร์ชั่นในเครื่องเราก่อน
npm --version

# ถ้าต้องการ สามารถสั่งติดตั้ง npm เวอร์ชั่น 6 สุดท้ายได้
npm i -g npm@6.14.12
```

## วิธีเตรียมเครื่องคอมพิวเตอร์

- [Windows](https://nextflow.in.th/2019/react-setup-for-windows-thai/)
- [MacOS](https://nextflow.in.th/2017/setup-mac-os-ios-react-native/)

## 1. สร้างโปรเจค 

```bash
expo init nextflow-note-app
```

## 2. ติดตั้ง Module 

```bash
npm install redux react-redux formik @react-navigation/native @react-navigation/stack native-base yup useeffectasync
```

หรือ

```bash
yarn add redux react-redux formik @react-navigation/native @react-navigation/stack native-base yup useeffectasync
```

## 3. ติดตั้ง Module สำหรับระบบ Navigation 

```bash
npx expo install expo-font expo-app-loading react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view
```




