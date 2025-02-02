
# 2. รันแอพพลิเคชั่น

## 1. ทดสอบรันโปรเจค 

1. เปิดโปรเจคขึ้นมาบน Terminal หรือ Command Prompt
2. รันคำสั่งด้านล่าง เพื่อรัน Expo Server

```bash
npx expo start
```

3. รอจนระบบพร้อมทำงาน
4. ทดสอบรันแอพผ่านคำสั่ง **Run on iOS simulator** (ทำได้บนเครื่อง MacOS ที่ติดตั้งโปรแกรมเรียบร้อยแล้วเท่านั้น)
5. ทดสอบรันแอพผ่านคำสั่ง **Run on Android Device/Emulator** (ต้องรัน Emulator จากโปรแกรม Android Studio > AVD Manager)

## 2. ทดสอบรันโปรเจคบนอุปกรณ์จริง

> เชื่อมต่ออุปกรณ์พกพาให้อยู่ในเครือข่ายเดียวกันกับคอมพิวเตอร์ที่รัน Expo Server อยู่

1. ติดตั้งแอพ [Expo for iOS](https://apps.apple.com/us/app/expo-client/id982107779), [Expo for Android](https://play.google.com/store/apps/details?id=host.exp.exponent&hl=th)
2. ทดสอบแสกนบาร์โค้ดด้วย แอพ Expo
3. หากติดปัญหาในการเชื่อมต่อ ให้ลองสลับการตั้งค่าในส่วน **Connection** จาก เป็น **Tunnel** หรือ **Local**

## 3. เตรียมพร้อมโปรเจคสำหรับการพัฒนา

1. ปิดการทำงานของ server โดยกลับมาที่ Terminal หรือ Command Prompt กด `Ctrl + C` แล้วกด `Y` เพื่อยืนยันการปิด
2. รันคำสั่งด้านล่าง เพื่อรีเซ็ตโปรเจคให้พร้อมสำหรับการพัฒนา

```bash
npm run reset-project
```

3. จะสังเกตว่าโครงสร้างของโปรเจคเปลี่ยนไป โดยจะมีโฟลเดอร์ชื่อ `app-example` แสดงขึ้นมา
4. เปิดดูโฟลเดอร์ `app` จะเห็นว่าเรามี `index.tsx` เป็นไฟล์เริ่มต้นของเรา