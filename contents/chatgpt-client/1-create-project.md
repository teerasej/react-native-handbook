
# 1. สร้างโปรเจค

## 1. สร้างโปรเจค 

1. คำสั่งสร้างโปรเจค
```bash
npm create gluestack@latest
```
2. เลือก template **Expo**
3. กำหนดชื่อโปรเจคเป็น

```bash
my-gpt-app
```
4. รอให้โปรเจคสร้างเสร็จ

### Troubleshooting 
1. หากเกิดข้อผิดพลาดในการสร้างโปรเจค สามารถใช้การ clone จากคำสั่ง git ด้านล่างได้
```bash
git clone --branch template-ready-react-native https://github.com/teerasej/nextflow-gluestack-react-native-my-app.git
```

2. หรือจะทำการ [download zip file จาก branch นี้](https://github.com/teerasej/nextflow-gluestack-react-native-my-app/tree/template-ready-react-native)ก็ได้เช่นกัน


## 2. ติดตั้ง Node Module 

1. ใช้คำสั่งเปิดเข้าไปใน directory ของโปรเจค
```bash
cd my-gpt-app
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





