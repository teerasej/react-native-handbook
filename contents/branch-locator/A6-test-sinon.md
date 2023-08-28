
# 6. test with sinon

เปิดไฟล์ `redux/actions.function.test.js`

```js

import actions from "./actions";
import sinon from "sinon";
import LocationService from "../services/location.service";

describe('Actions Function', () => {
    
    it('Should use getLocation from LocationService', () => {

        // ใช้ sinon จำลอง function ชื่อ getLocation ของ LocationService module
        const getLocation = sinon.stub(LocationService, 'getLocation');

        // ให้ function คืนค่า async เป็น { location: {} }
        getLocation.resolves({ location: {} });

        // เริ่มการทำงานจริง
        actions.getCurrentLocation(undefined);

        // อย่าลืมสั่ง restore sinon ทุกครั้ง
        getLocation.restore();

        // ทดสอบว่ามีการเรียกใช้ 1 ครั้งในการทำงาน
        sinon.assert.calledOnce(getLocation);
       
    });


});
```

จะเทสได้ ต้องไปเปลี่ยนโครงสร้าง module `services/location.service.js`

```js
import * as Location from 'expo-location';
import * as Permissions from 'expo-permissions';

export const getLocation = async () => {

    let { status } = await Permissions.askAsync(Permissions.LOCATION);

    if (status !== 'granted') {
        return {
          errorMessage: 'Permission to access location was denied',
        };
      }

    let currentLocation = await Location.getCurrentPositionAsync({ accuracy: Location.Accuracy.Highest });
    return {
      location: currentLocation
    };

}

export default {
  getLocation: getLocation
}
```

และใน `redux/actions.js`

```js
import locationService from "../services/location.service"

const getCurrentLocation = async (dispatch) => {

    const {location, errorMessage } = await locationService.getLocation();
    
    if(location){
        dispatch({
            type: Types.LOCATION_DEVICE_FOUND,
            payload: location
        })
    } else if (errorMessage) {
        console.error(errorMessage);
    }

}
```