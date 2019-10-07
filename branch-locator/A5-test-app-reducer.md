
# 5. Test App.reducer

สร้างไฟล์​ `redux/reducers/app.reducer.test.js`

```js
import appReducer from "./app.reducer";
import actions from "../actions";

describe('App Reducer', () => {
    
    it('Should Exist', () => {
        expect(appReducer).toBeDefined()
    });

    it('Location should added', () => {

        const initialState = {};

        const action = {
            type: actions.Types.LOCATION_DEVICE_FOUND,
            payload: { latitude: 1.0, longitude: 2.0 }
        }

        const result = appReducer(undefined, action);

        expect(result.currentLocation).toBeDefined();
        expect(result.currentLocation.latitude).toBe(1.0);
        expect(result.currentLocation.longitude).toBe(2.0)
    });


});
```

ทีนี้มาลองเทสแบบที่ต้อง failed บ้าง 

```js

    it('Empty object should not be added as location', () => {

        const initialState = {};

        const action = {
            type: actions.Types.LOCATION_DEVICE_FOUND,
            payload: {}
        }

        const result = appReducer(undefined, action);

        expect(result.currentLocation).toBeUndefined();
    });
```