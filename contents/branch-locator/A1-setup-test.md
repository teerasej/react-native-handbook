
# 1. Setup test environment

- [ดาวน์โหลดโปรเจคสมบูรณ์](https://www.dropbox.com/s/47xa8n2e6a2wv7a/react-native-branch-map-03-load-branch-data.zip?dl=0)

## 1. ติดตั้ง module

```bash
npm i jest jest-expo sinon react-test-renderer --save-dev
```

หรือ 

```bash
yarn add jest jest-expo sinon react-test-renderer --dev
```

## 2. เพิ่ม setting ใน package.json 

เปิดไฟล์ `package.json`

```json
"jest": {
    "preset": "jest-expo",
    "transformIgnorePatterns": [
      "node_modules/(?!(jest-)?react-native|react-clone-referenced-element|@react-native-community|expo(nent)?|@expo(nent)?/.*|react-navigation|@react-navigation/.*|@unimodules/.*|sentry-expo|native-base)"
    ]
  }
```

## 3. ทดสอบรัน test

เพิ่ม script "test" ลงใน `package.json`

```json
"scripts": {
    ...
    "test": "jest --watchAll"
},
```

และทดสอบโดยรันคำสั่ง 

```bash
npm test
```

น่าจะเห็น log แสดงขึ้นมา แสดงถึง jest คอยรัน test unit ของเรา

```bash
No tests found, exiting with code 0

Watch Usage
 › Press f to run only failed tests.
 › Press o to only run tests related to changed files.
 › Press p to filter by a filename regex pattern.
 › Press t to filter by a test name regex pattern.
 › Press q to quit watch mode.
 › Press Enter to trigger a test run.
```