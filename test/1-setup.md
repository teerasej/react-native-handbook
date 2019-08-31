
# Setup Test

## 1. ติดตั้ง Test module 

```bash
npm i enzyme enzyme-adapter-react-16 jest-expo jest-enzyme enzyme-react-16-adapter-setup react-test-renderer redux-saga-test-plan --save-dev
```

หรือ

```bash
yarn add enzyme enzyme-adapter-react-16 jest-expo enzyme-react-16-adapter-setup react-test-renderer redux-saga-test-plan --dev
```

### *** Version Compatibility ***

ควรเช็คให้แน่ใจว่า package `react`, `react-dom`, และ `react-test-renderer` เป็นเวอร์ชั่นเดียวกัน เช่น 

```json
// package.json 
"dependencies": {
    "react": "16.8.3",
    "react-dom": "16.8.3",
},
"devDependencies": {
    "react-test-renderer": "16.8.3"
}
```

ไม่งั้นอาจจะเจออาการแบบนี้ตอน run ตัวเทส 

```bash
TypeError: Cannot read property 'current' of undefined
```

## 2. ตั้งค่า package.json 

เพิ่มค่าด้านล่างเข้าไปในไฟล์ package.json 

```json
"scripts": {
  ...
  "test": "jest --watch --coverage=false --changedSince=origin/master",
  "testDebug": "jest -o --watch --coverage=false",
  "testFinal": "jest",
  "updateSnapshots": "jest -u --coverage=false"
},
"jest": {
    "preset": "jest-expo"
}
```
