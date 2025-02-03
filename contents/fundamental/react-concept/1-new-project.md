
# 2. Create Project

## 1. สร้างโปรเจค 
1. คำสั่งสร้างโปรเจค
```bash
npm create gluestack@latest
```
หรือ
```bash
 npx create-gluestack@latest
```
หรือ ทำการ clone project จาก URL ด้านล่าง
```
https://github.com/teerasej/nextflow-gluestack-react-native-my-app
```

### Error

ถ้าเจอ error ประมาณนี้ บน Windows 

```
error: invalid path 'apps/templates/universal-with-nativewind/frontend/packages/modules/aux/components/PrivacyPolicy.tsx'
```
ให้รันคำสั่งนี้บน Terminal
```
git config core.protectNTFS false
```

2. เลือก template **Expo**
3. กำหนดชื่อโปรเจคเป็น

```bash
nextflow-my-app
```

4. รอให้โปรเจคสร้างเสร็จ


## 2. ติดตั้ง Node Module 

1. ใช้คำสั่งเปิดเข้าไปใน directory ของโปรเจค
```bash
cd nextflow-my-app
```

2. ติดตั้ง Node Module ด้วยคำสั่ง
```bash
npm install @types/react @types/react-native --save-dev
```

3. เปิดไฟล์ `tsconfig.json` และเพิ่ม `"jsx": "react-native"` ในส่วน `compilerOptions`

```json
{
  "extends": "expo/tsconfig.base",
  "compilerOptions": {
    "strict": true,
    "paths": {
      "@/*": ["./*"],
      "tailwind.config": ["./tailwind.config.js"]
    },
    "jsx":"react-native"
  },
  "include": [
    "**/*.ts",
    "**/*.tsx",
    ".expo/types/**/*.ts",
    "expo-env.d.ts",
    "nativewind-env.d.ts"
  ]
}
```


### Troubleshooting

1. ถ้าเจอปัญหาเกี่ยวกับ peer dependency ให้ใช้คำสั่ง
```bash
npm config set legacy-peer-deps true  
```
2. ติดตั้ง Node Module ด้วยคำสั่ง
```bash
npm install @react-navigation/native @react-navigation/native-stack native-base
```

หรือ

```bash
yarn add @react-navigation/native @react-navigation/native-stack native-base
```

### Reference

- Nativebase

## 3. ติดตั้ง package สำหรับใช้งาน Nativebase 

```bash
npx expo install react-native-svg react-native-safe-area-context expo-font react-native-screens
```





