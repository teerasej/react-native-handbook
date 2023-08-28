

# 2. รันแอพพลิเคชั่น

## 1. เปิดการทำงานของ Expo Server

1. เปิดโปรเจคขึ้นมาบน Terminal 
2. รันคำสั่งด้านล่าง เพื่อรัน Expo Server สำหรับเชื่อมต่อในวง LAN เดียวกันระหว่างเครื่องที่รันโปรเจค และมือถือที่ลงแอพ Expo Go

```bash
npx expo start
```

** ถ้าไม่ได้อยู่ในวง LAN เดียวกัน หรือมีปัญหาในการ block port จนหากันไม่เจอ ให้รันคำสั่งด้านล่างแทน เพื่อเชื่อมต่อกันด้วย ngrok

```bash
npx expo start --tunnel
```

หรือถ้าไม่สามารถรันคำสั่งที่ใช้ `npx expo` ได้ ให้รันคำสั่งด้านล่างแทน

```bash
npm start
หรือ 
yarn start
```

3. รอจนระบบพร้อมทำงาน จะมีหน้าเว็บถูกเปิดขึ้นมา และมี QR code แสดงในบริเวณด้านซ้าย

### A. การรันทดสอบ บน iOS Simulator

เฉพาะ macOS [ที่ติดตั้งโปรแกรมพร้อมแล้วเท่านั้น](https://nextflow.in.th/2017/setup-mac-os-ios-react-native/)

1. จากหน้าเว็บของ Expo 
2. ทดสอบรันแอพ ผ่านการกดปุ่มคำสั่ง **Run on iOS simulator**
3. รอจน iOS Simulator ถูกเปิดขึ้นมา
4. หากเป็นการรันครั้งแรก จะมีการดาวน์โหลดแอพ Expo มาติดตั้งใน Simulator อัตโนมัติ
5. แอพ Expo บน Simulator จะพยายาม sync โค้ดจากคอมพิวเตอร์ของเรา
6. สามารถใช้ปุ่ม `Cmd + Crl + Z` เพื่อเปิดเมนูควบคุมได้ เช่นการบังคับ Reload โค้ดจากโปรเจค

### B. การรันทดสอบ บน Android Emulator

หากยังไม่ได้เปิด Emulator ต้องรัน Emulator จากโปรแกรม Android Studio > AVD Manager > เลือกสร้าง หรือรัน Emulator ใช้งาน

1. ทดสอบรันแอพ ผ่านการกดปุ่มคำสั่ง **Run on Android Device/Emulator**
2. หากเป็นการรันครั้งแรก จะมีการดาวน์โหลดแอพ Expo มาติดตั้งใน Emulator อัตโนมัติ
3. แอพ Expo บน Simulator จะพยายาม sync โค้ดจากคอมพิวเตอร์ของเรา
4. สามารถใช้ลากแถบ Notification Center จากขอบจอด้านบนลงมา เพื่อเปิดเมนูควบคุมได้ เช่นการบังคับ Reload โค้ดจากโปรเจค

## 2. ทดสอบรันโปรเจคบนอุปกรณ์จริง

> เชื่อมต่ออุปกรณ์พกพาให้อยู่ในเครือข่ายเดียวกันกับคอมพิวเตอร์ที่รัน Expo Server อยู่

1. ติดตั้งแอพ [Expo for iOS](https://apps.apple.com/us/app/expo-client/id982107779), [Expo for Android](https://play.google.com/store/apps/details?id=host.exp.exponent&hl=th)

สามารถค้นหาแอพชื่อ **Expo** ใน App Store และ Play Store ได้

2. ทดสอบแสกนบาร์โค้ด ด้วย แอพ Expo
3. แอพ Expo จะพยายาม sync โค้ดจากคอมพิวเตอร์ของเรา ในที่นี้ควรให้แน่ใจว่าคอมพิวเตอร์ และอุปกรณ์ของเรา อยู่บนเครือข่ายเดียวกัน
4. หากติดปัญหาในการเชื่อมต่อ ให้ลองสลับการตั้งค่าในส่วน **Connection** จาก เป็น **Tunnel** หรือ **Local**

## 3. ทดสอบรันแบบ Web 

> ⚠️ ในการรันแบบ Web ตัว UI component ที่นำมาใช้ต้องรองรับการรันแบบเว็บด้วยนะ ให้ศึกษาเพิ่มเติมในคู่มือของ UI นั้นๆ ครับ

1. หลังจากรันคำสั่ง `npx expo start` แล้ว จะสังเกตจากเมนูของ Expo server ว่า เราสามารถกดปุ่มลัดชื่อ `w` เพื่อรันแบบ web 
2. ถ้ากดปุ่มแล้วระบบพบว่า ต้องมีการติดตั้ง package เพิ่มเติม จะมีการแสดงข้อความมาประมาณด้านล่าง 
   
```
It looks like you're trying to use web support but don't have the required dependencies installed.

Please install react-native-web@~0.19.6, react-dom@18.2.0, @expo/webpack-config@^19.0.0 by running:

npx expo install react-native-web@~0.19.6 react-dom@18.2.0 @expo/webpack-config@^19.0.0

If you're not using web, please ensure you remove the "web" string from the platforms array in the
project Expo config.
```

3. ปิด Expo server ลงโดยใช้ปุ่ม Ctrl + C หรือ Cmd + C 
4. หลังจากปิด Expo server แล้ว เราสามารถเอาคำสั่ง `npx expo install ...` ที่แสดงขึ้นมาในคำแนะนำด้านบนมารัน เพื่อติดตั้ง package ที่จำเป็น เช่นในตัวอย่างคือ 

```
npx expo install react-native-web@~0.19.6 react-dom@18.2.0 @expo/webpack-config@^19.0.0
```
> ให้แน่ใจว่าได้ใช้คำสั่งจากที่แสดงในเครื่องของตัวเอง ห้าม copy คำสั่งด้านบนไปใช้ เพราะเลขเวอร์ชั่นจะเปลี่ยนไปตามความเหมาะสมของระบบตอนนั้น

5. หลังจากรันคำสั่งติดตั้งเรียบร้อย ให้ใช้คำสั่ง `npx expo start` ขึ้นมาอีกครั้ง
6. หลังจาก Expo server เปิดทำงานเสร็จสมบูรณ์ ให้กดปุ่ม w เพื่อรันแบบ Web 







## หากติดปัญหาในการเชื่อมต่อ 

ให้ลองสลับการตั้งค่าในส่วน **Connection** จาก เป็น **Tunnel** หรือ **Local**

