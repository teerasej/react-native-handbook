
# 2. การสร้างไฟล์สำหรับ Android (Export for Android)

> [App Signing for Android](https://docs.expo.io/versions/latest/distribution/app-signing/)



## 1. รันคำสั่งสร้างไฟล์ Android App Bundle  

รันคำสั่ง ด้านล่างเพื่อเริ่มกระบวนการ 

```bash
expo build:android -t app-bundle --clear-credentials
```

หรือสำหรับการสร้างไฟล์ APK 

```bash
expo build:android -t apk --clear-credentials
```

และเลือกตัวเลือก `Let Expo handle the process!` เพื่อให้ Expo 

1. จัดการสร้าง keystore และ sign ไฟล์ APK
2. ระบบจะอัพโหลดไฟล์ขึ้นไป build บนระบบของ Expo 
3. จะมี URL เพื่อติดตามการ build และดาวน์โหลดไฟล์ที่เสร็จสมบูรณ์ตามตัวอย่าง

```bash
You can monitor the build at

https://expo.io/builds/03641c65-f236-43eb-8860-4c4a2bc2165b

Waiting for build to complete. You can press Ctrl+C to exit.
✔ Build finished.
Successfully built standalone app: https://expo.io/artifacts/e5cbb0dd-a798-4dd2-95c5-fca3fd873137
```

## 2. สร้างไฟล์ Certificate และ Keystore 

### Certificate

รันคำสั่งสร้างไฟล์​ Certificate (ส่วนนี้สามารถเอาไปอัพโหลดขึ้น Google Play Console ได้)

```bash
fetch:android:upload-cert
```

เราจะได้ไฟล์นามสกุล `.pem` ที่ต้องอัพโหลดไปกับการ release เวอร์ชั่นใหม่ของแอพพลิเคชั่นทุกครั้ง 

### Keystore 

รันคำสั่งด้านล่าง เพื่อดาวน์โหลด keystore ที่ Expo สร้างให้มาเก็บไว้

```bash
expo fetch:android:keystore
```

## 3. รันคำสั่ง build:android ปกติ ถ่้าต้องการ build ใหม่ 

```bash
expo build:android -t app-bundle
```

หรือ 

```bash
expo build:android -t apk
```
