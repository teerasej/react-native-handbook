
# Setup your machine 

## ⚠️ ข้อควรทราบ ด้าน Network และ Security

- การอบรมจะมีการรันคำสั่งผ่าน โปรแกรม Command Prompt และมีโปรเซสที่ทำการสร้างและแก้ไขไฟล์ที่อยู่บนระบบปฏิบัติการ (เช่น Node process) ให้แน่ใจว่าระบบปฏิบัติการไม่ได้มีการบล๊อคการทำงาน และสามารถทำงานได้อย่างไม่มีปัญหา
- **แนะนำว่าสำหรับองค์กรที่มีการ block การทำงานผ่านอินเตอร์เน็ต ควรเตรียม Account สำหรับเชื่อมต่ออินเตอร์เน็ตวงนอกให้ผู้เข้าอบรมได้เลย**
- ระหว่างการอบรม จะมีการเชื่อมต่ออินเตอร์เน็ต และมีการดาวน์โหลดโค้ด รวมถึงติดตั้งโมดูลเพิ่มเติม ควรให้แน่ใจว่าคอมพิวเตอร์ทีี่ใช้ไม่มีการปิดกั้นการทำงานดังกล่าว 

## สำหรับ Computer

ทำการดาวน์โหลดตัวติดตั้ง และดำเนินการติดตั้งให้เรียบร้อย

1. Node Runtime รุ่น LTS ([Download](https://nodejs.org/en/download) | [คลิปแนะนำการติดตั้ง](https://www.youtube.com/watch?v=GET7GPha6gM))
2. Git Client ([Download](http://git-scm.com/download/) | [คลิปแนะนำการติดตั้ง](https://www.youtube.com/watch?v=fPOoIZbDKmE))
3. Visual Studio Code ([Download](https://code.visualstudio.com/))
   - เสร็จแล้วติดตั้งส่วนเสริม 
     1. [Nextflow React Native Extension Pack](https://marketplace.visualstudio.com/items?itemName=teerasej.nextflow-react-native-pack) 
     2. [Expo Tools](https://marketplace.visualstudio.com/items?itemName=expo.vscode-expo-tools)


### การติดตั้งระบบ

เปิดโปรแกรม Terminal (MacOS) หรือ Command Prompt (Windows 11) และรันคำสั่งด้านล่าง ทีละคำสั่ง

#### Windows 11+

```bash
npm install -g expo
npm install -g @expo/ngrok@^4.1.0
npm install -g eas-cli
```

#### MacOS

1. ติดตั้ง[โปรแกรม Xcode ผ่าน App Store](https://apps.apple.com/us/app/xcode/id497799835?mt=12/)
2. รันคำสั่งด้านล่าง ในโปรแกรม Terminal ทีละคำสั่ง

```bash
sudo npm install -g expo
sudo npm install -g @expo/ngrok@^4.1.0
sudo npm install -g eas-cli
```

3. ทำการติดตั้ง homebrew ผ่านโปรแกรม Terminal ด้วยคำสั่งที่อยู่ใน[เว็บ Homebrew นี้](https://brew.sh/index_th)
4. รันคำสั่งด้านล่าง ในโปรแกรม Terminal ทีละคำสั่ง

```bash
brew cask install fastlane
```

## สำหรับอุปกรณ์พกพา

อุปกรณ์พกพาจะถูกใช้ในการพรีวิว และทดสอบแอพพลิเคชั่นที่กำลังพัฒนาบนคอมพิวเตอร์ 

- [ระบบ iOS](https://itunes.apple.com/app/apple-store/id982107779?ct=www&mt=8)
- [ระบบ Android](https://play.google.com/store/apps/details?id=host.exp.exponent&referrer=www)

