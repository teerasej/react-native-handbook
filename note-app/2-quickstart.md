
# 2. รันแอพพลิเคชั่น

## 1. ทดสอบรันโปรเจค 

1. เปิดโปรเจคขึ้นมาบน Terminal 
2. รันคำสั่งด้านล่าง เพื่อรัน Expo Server

```bash
npx expo start
```

3. รอจนระบบพร้อมทำงาน
4. ทดสอบรันแอพผ่านคำสั่ง **Run on iOS simulator**
5. ทดสอบรันแอพผ่านคำสั่ง **Run on Android Device/Emulator** (ต้องรัน Emulator จากโปรแกรม Android Studio > AVD Manager)

## 2. ทดสอบรันโปรเจคบนอุปกรณ์จริง

> เชื่อมต่ออุปกรณ์พกพาให้อยู่ในเครือข่ายเดียวกันกับคอมพิวเตอร์ที่รัน Expo Server อยู่

1. ติดตั้งแอพ [Expo for iOS](https://apps.apple.com/us/app/expo-client/id982107779), [Expo for Android](https://play.google.com/store/apps/details?id=host.exp.exponent&hl=th)
2. ทดสอบแสกนบาร์โค้ดด้วย แอพ Expo
3. หากติดปัญหา ให้ลองสลับโหมด **Connection** เป็น **Tunnel** หรือ **Local**

