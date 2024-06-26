
# Setup your machine 

- [Advanced Topics Exercise](/contents/advanced-topics/README.md)

ให้ทำตามครบทุกขั้นตอนเพื่อให้แน่ใจว่า ระบบและซอฟต์แวร์สามารถทำงานได้อย่างไม่มีปัญหา **และควรทำล่วงหน้าก่อนวันอบรมสักอย่างน้อย 4-5 วัน เพราะถ้าพบว่าเครื่องของตัวเองถูก block หรือจำกัดสิทธิ์ จะได้ทำเรื่องกับฝ่าย Security หรือ Admin เพื่อดำเนินการให้สามารถติดตั้งและใช้งานได้ตามปกติ**

## ⚠️ ข้อควรระวัง เรื่องการใช้เครื่องคอมขององค์กร มาใช้ในการอบรม

- **แนะนำว่าสำหรับองค์กรที่มีการ block การทำงานผ่านอินเตอร์เน็ต เช่นการ block port หรือ certificate ให้เตรียม Account สำหรับเชื่อมต่ออินเตอร์เน็ตวงนอกให้ผู้เข้าอบรมได้เลย**
- การอบรมจะมีการรันคำสั่งผ่าน โปรแกรม Command Prompt หรือ Terminal และมีโปรเซสที่ทำการสร้างและแก้ไขไฟล์ที่อยู่บนระบบปฏิบัติการ (เช่น NodeJS process) ให้แน่ใจว่าระบบปฏิบัติการไม่ได้มีการบล๊อคการทำงาน และสามารถทำงานได้อย่างไม่มีปัญหา
- ระหว่างการอบรม จะมีการเชื่อมต่ออินเตอร์เน็ต และมีการดาวน์โหลดโค้ด รวมถึงติดตั้ง, setup, และ install โปรแกรมเพิ่มเติม **ควรให้แน่ใจว่าคอมพิวเตอร์ทีี่ใช้ไม่มีการปิดกั้นการทำงานดังกล่าว**

> ⚠️⚠️⚠️ หากทางผู้ดูแลระบบไม่ได้มีการอำนวยความสะดวกในการผ่อนปรนตามสถานการณ์ด้านบน ทางผู้เข้าอบรมอาจจะไม่สามารถใช้คอมพิวเตอร์นั้นเรียนรู้ หรือทำ workshop บางส่วนได้ จึงขอแจ้งมาเพื่อทราบครับ

## 1. สำหรับ Computer ที่ใช้ในการทำงาน

ทำการดาวน์โหลดตัวติดตั้ง และดำเนินการติดตั้งให้เรียบร้อย

1. Node Runtime รุ่น LTS ([Download](https://nodejs.org/en/download) | [คลิปแนะนำการติดตั้ง](https://www.youtube.com/watch?v=GET7GPha6gM))
2. Git Client ([Download](http://git-scm.com/download/) | [คลิปแนะนำการติดตั้ง](https://www.youtube.com/watch?v=fPOoIZbDKmE))
3. Visual Studio Code ([Download](https://code.visualstudio.com/))
   - เสร็จแล้วติดตั้งส่วนเสริม 
     1. [Nextflow React Native Extension Pack](https://marketplace.visualstudio.com/items?itemName=teerasej.nextflow-react-native-pack) 
     2. [Expo Tools](https://marketplace.visualstudio.com/items?itemName=expo.vscode-expo-tools)
4. Web Browser สามารถเลือกระหว่าง 2 อย่างนี้ 
   1. Google Chrome ([Download](https://www.google.com/chrome/))
   2. Microsoft Edge ([Download](https://www.microsoft.com/en-us/edge/download))
5. แล้วก็ติดตั้ง Extension บน Web Browser ตามรายการต่อไปนี้ 
   1. [React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools)
   2. [Redux Dev Tools](https://chrome.google.com/webstore/detail/redux-devtools)

## 2. สมัคร Account ที่ควรมี

[สมัคร Github Account](https://github.com/signup) ให้เรียบร้อย

## 3. การติดตั้งระบบ

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

## 4. สำหรับอุปกรณ์พกพา

อุปกรณ์พกพาจะถูกใช้ในการพรีวิว และทดสอบแอพพลิเคชั่นที่กำลังพัฒนาบนคอมพิวเตอร์ 

- [ระบบ iOS](https://itunes.apple.com/app/apple-store/id982107779?ct=www&mt=8)
- [ระบบ Android](https://play.google.com/store/apps/details?id=host.exp.exponent&referrer=www)


## 5. ทดสอบการติดตั้ง 

[คลิกดูวิธีทดสอบการสร้างโปรเจค และพรีวิวบนมือถือที่นี่ ](https://nextflow.in.th/2023/react-native-validate-setup-machine/)

หากพบว่าไม่สามารถทำได้ตามขั้นตอน แปลว่า อาจจะยังติดตั้งไม่ครบ หรือมีการ block การทำงานของแอพ, ระบบ, หรือ port ในการพรีวิวแอพ ให้ติดต่อผู้ดูแลระบบเพื่อดำเนินการครับ
