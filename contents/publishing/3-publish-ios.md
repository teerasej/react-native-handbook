
# การสร้างไฟล์สำหรับ iOS (Export for iOS)

> [อ้างอิง Export for iOS](https://docs.expo.io/versions/latest/distribution/building-standalone-apps/#if-you-choose-to-build-for-ios) 

## ขั้นตอนทั่วไป

### 1. รันคำสั่ง build android 

```
eas build -p ios
```

### 2. กำหนดข้อมูลตามขั้นตอน

ตั้งชื่อ Bundle Identifier

```
? What would you like your iOS bundle identifier to be? … com.teerasej.nextflowchatgptapp
```

Login Apple Id เพื่อดึงข้อมูลที่จำเป็นมาใช้ในการ build

```
If you provide your Apple account credentials we will be able to generate all necessary build credentials and fully validate them.
This is optional, but without Apple account access you will need to provide all the missing values manually and we can only run minimal validation on them.
✔ Do you want to log in to your Apple account?
```

### 3. รอให้ระบบ build จนเสร็จ

ใช้คำสั่งด้านล่างเพื่อดูรายการ build ที่ทำงานอยู่ได้

```bash
eas build:list
```

ใช้คำสั่งด้านล่าง เพื่อเปิดไปยัง Dashboard เพื่อดูสถานะของการ build และดาวน์โหลดไฟล์ได้ 

```bash
eas build:dashboard
```
