
# 4. สร้าง Redux Store



## Redux Store

ไฟล์ส่วนที่รวมการทำงานของทุกส่วนใน Redux เข้าด้วยกัน ให้มองว่าเป็น Global Setting หรือ main board ใน computer ก็ได้ 

ในที่นี้สร้างเป็นไฟล์แยก เพื่อการปรับแต่งการทำงานได้ง่าย

## สร้าง Redux Store

สร้างไฟล์ `redux/store.js`

```js
import { createStore } from 'redux'
import reducer from "./reducer"

export default configureStore = () => {
    const store = createStore(reducer)
    return store
}   
```

