
# 4. สร้าง Redux Store



## Redux Store

ไฟล์ส่วนที่รวมการทำงานของทุกส่วนใน Redux เข้าด้วยกัน ให้มองว่าเป็น Global Setting หรือ main board ใน computer ก็ได้ 

ในที่นี้สร้างเป็นไฟล์แยก เพื่อการปรับแต่งการทำงานได้ง่าย

## สร้าง Redux Store

สร้างไฟล์ `redux/store.js`

```js
import { createStore, combineReducers } from 'redux';

import noteReducer from "./note.reducer";
import { reducer as formReducer } from "redux-form";

export default configureStore = () => {
    const store = createStore(
        // `combineReducers` เป็น function สำหรับการรวม Reducer ต่างๆ เข้าด้วยกัน
        combineReducers({
            // NoteReducer เป็น function ที่เราสร้างไว้
            note: noteReducer,
            // จะเห็นว่าในที่นี้เรามี Form Reducer ที่เอาไว้จัดการข้อมูลจาก Form ด้วย
            form: formReducer
        })
    );

    return store;
}   
```

