
# 2. Test store 

> https://jestjs.io/docs/en/expect

สร้างไฟล์ `redux/store.test.js`

```js

import configureStore from "./store";

describe('Redux Store', () => {
    
    it('Exist', () => {
        const store = configureStore();

        // store ต้องไม่ใช่ null
        expect(store).toBeDefined();
    });

});
```

ทดสอบรันเทส