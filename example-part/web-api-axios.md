
# การใช้งาน Axios ใน Redux

Axios เป็น module หนึ่งที่ได้รับความนิยมในการติดต่อกับ Web API ซึ่งมีการปรับปรุงการทำงานเพิ่มเติมจาก `fetch()`

## ดาวน์โหลดโปรเจค Workshop 

ใช้คำสั่ง git clone โปรเจคมาไว้ในเครื่อง และรันคำสั่ง `npm i` หรือ `yarn` ให้เรียบร้อย

```bash
git clone -b start-get-user https://github.com/teerasej/react-native-random-user-contact
```


## การติดตั้ง Axios

```bash
npm i axios
```
หรือ
```bash
yarn add axios
```

## การ Import Axios มาใช้งาน


ปรับใช้ Axios ใน `redux/action-start-get-user.js`


```js

import axios from 'axios'

//...

const startGetUser = async (dispatch) => {
    
    const url = 'https://randomuser.me/api/?results=50';

    // axios จะมี method ที่สร้างมาเพื่อความสะดวกในการติดต่อกับ web api เช่น .get()
    const response = await axios.get(url);

    if (response && response.status === 200) {

        // AxiosResponse จะมี data property ที่แปลง json มาใช้งานได้ทันที 
        const json = await response.data;

        console.log('JSON:',json);
        console.log('Accounts:', json.results);

        dispatch({
            type: Types.GET_USERS_SUCCESS,
            payload: json.results
        });

    } else {
        console.error(response.status, response);
    }
    
}
```