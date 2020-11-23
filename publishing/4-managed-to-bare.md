
# การแปลง Expo Managed Workflow ไปเป็น bare workflow

Managed workflow นั้นอำนวยความสะดวกมากมายให้กับนักพัฒนา React Native แต่ถ้าต้องการ เราสามารถ ปลด โปรเจคเข้าสู่ bare workflow ได้ 

## 1. ปลด expo ออกจาก 

```bash
expo eject
```

ในที่นี้ระบบ expo จะทำการสร้างโปรเจค Android และ iOS ขึ้นมา 
และมีการถาม id ของแอพ ซึ่งเราต้องทำการกำหนดระหว่างการดำเนินการ

เช่น

```
th.in.nextflow.react.native.contactapp
```

## 2. รันคำสั่ง publish

การรันคำสั่ง publish จะทำให้โปรเจคหลังจากถอด Managed Workflow เข้าสู่สภาพพร้อมใช้งาน

```
expo publish
```

## 3. การกำหนด Icon และ Splash Screen

[สามารถอ้างอิงตัวอย่างได้จาก Medium](https://medium.com/better-programming/react-native-add-app-icons-and-launch-screens-onto-ios-and-android-apps-3bfbc20b7d4c)

