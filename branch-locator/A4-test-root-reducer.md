
# Test Root Reducer

สร้างไฟล์​ `redux/reducers/root.reducer.test.js` 

```js
import combinedReducer from "./root.reducer";

describe('Combined Reducer', () => {
    
    it('Should has only 2 reducers', () => {
        const reducers = combinedReducer();
        
        expect(reducers.length).toBe(2);
    });

});
```