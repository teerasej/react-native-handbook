
# Build Locally

หากต้องการ build ตัวโปรเจค

## คำสั่งสร้าง Native project directory

คำสั่งด้านล่างจะเป็นการ setup directory สำหรับ iOS และ Android ขึ้นมาในโปรเจค

```bash
npx expo prebuild
```

หากมีการแก้ไขโปรเจค ให้เราทำการสั่งคำสั่งด้านบนอีกครั้ง เพื่ออัพเดตโค้ดใน Native project directory

หากต้องการให้ล้าง Native directory ใหม่หมดจดให้ใช้คำสั่งด้านล่าง 

```bash
npx expo prebuild --clean
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

