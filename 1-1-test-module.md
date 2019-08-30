## ติดตั้ง Test module 

```bash
npm i enzyme enzyme-adapter-react-16 jest-expo react-test-renderer --save-dev
```

หรือ

```bash
enzyme enzyme-adapter-react-16 jest-expo react-test-renderer --dev
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
