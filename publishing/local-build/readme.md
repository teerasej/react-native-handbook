
# Build Locally

หากต้องการ build ตัวโปรเจคแบบ

## ติดตั้ง eas cli

1. ต้องผ่านการติดตั้งระบบ และโปรแกรมพื้นฐาน ตามที่มี[ในขั้นตอน setup](../../setup.md)ให้เรียบร้อยก่อน
2. เปิดโปรแกรม Terminal (MacOS) หรือ Command Prompt (Windows 11) และรันคำสั่งด้านล่าง 

### Windows 11+

```bash
npm install -g eas-cli
```

### MacOS

```bash
sudo npm install -g eas-cli
```

## การ Setup ส่่วนประกอบสำหรับการ build local

### Windows - Android 

1. [การติดตั้ง Chocolatey](https://learn.nextflow.in.th/view/courses/google-flutter/2052156-windows/6454118-chocolatey)
2. [การติดตั้ง JDK ผ่าน Chocolatey](https://learn.nextflow.in.th/view/courses/google-flutter/2052156-windows/6454539-java-development-kit-jdk)
3. [การติดตั้ง Android Studio บน Windows 11](https://learn.nextflow.in.th/view/courses/google-flutter/2052156-windows/6454545-android-studio-windows)

### MacOS - iOS & Android

1. [การติดตั้ง Homebrew](https://learn.nextflow.in.th/view/courses/google-flutter/2052286-macos/6454557-homebrew)
2. [การติดตั้ง Java Development Kit (JDK) ผ่าน Homebrew](https://learn.nextflow.in.th/view/courses/google-flutter/2052286-macos/6454573-java-development-kit-homebrew)
3. [วิธีติดตั้ง Android Studio และ Android SDK บน MacOS](http://learn.nextflow.in.th/view/courses/google-flutter/2052286-macos/6455275-android-studio-android-sdk-macos)
4. [วิธีการติดตั้ง Xcode](http://learn.nextflow.in.th/view/courses/google-flutter/2052286-macos/6457332-xcode)

## คำสั่ง build ไฟล์แอพพลิเคชั่นบนเครื่องของเราโดยตรง


### Android

```bash
eas build --platform android --local
```

### iOS 

```bash
eas build --platform ios --local
```



## การกำหนด Android package และ iOS Bundle Identifier

ในตอนที่สั่ง prebuild ครั้งแรก จะมีการให้ตั้งชื่อ 2 ชื่อ 

```bash
📝  Android package Learn more

✔ What would you like your Android package name to be? … com.teerasej.nextflow.app


📝  iOS Bundle Identifier Learn more

✔ What would you like your iOS bundle identifier to be? … com.teerasej.nextflow.app
```

การตั้งชื่อ Android package และ iOS Bundle Identifier จะใช้เทคนิคการเขียนชื่อ domain name ของเว็บไซต์ของตัวเองกลับหลัง (reverse domain name) เพื่อให้ซ้ำกับของคนอื่นได้ยาก เช่น

```
nextflow.in.th
```

ก็สามารถตั้งเป็น

```
th.in.nextflow.app.hr.mainapp
```

## เพ่ิมเติม

https://docs.expo.dev/workflow/prebuild/ 

