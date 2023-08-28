
# 5. สร้าง action function ที่ค้นหาตำแหน่ง

เปิดไฟล์ `redux/actions.js`

```js
import locationService from "../services/location.service"

//... 

const getCurrentLocation = async (dispatch) => {

    // ดึงค่า location, errorMessage มาจากการของตำแหน่ง
    const { location, errorMessage } = await locationService();

    // ถ้ามีตำแหน่งคืนกลับมา ส่งเข้า action ไปให้ reducer
    if(location){
        dispatch({
            type: Types.LOCATION_DEVICE_FOUND,
            payload: location
        })
    } else if (errorMessage) {
        // ถ้าไม่ใช่ให้แสดงเป็น error
        console.error(errorMessage);
    }

}
```

## A. ไฟล์เต็ม `redux/actions.js`

```js
import locationService from "../services/location.service"


const Types = {
   LOCATION_DEVICE_FOUND: 'LOCATION_DEVICE_FOUND'
}

const getCurrentLocation = async (dispatch) => {

    const {location, errorMessage } = await locationService();

    if(location){
        dispatch({
            type: Types.LOCATION_DEVICE_FOUND,
            payload: location
        })
    } else if (errorMessage) {
        console.error(errorMessage);
    }

}

export default {
    Types,
    getCurrentLocation
}
```