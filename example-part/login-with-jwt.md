
# Login with JWT 

- [Download Starter Project](https://www.dropbox.com/s/jhm483ec1obeifa/react-native-branch-map-starter.zip?dl=0)
- [Download Finish Project](https://www.dropbox.com/s/ok1094ayq41ytya/react-native-branch-map-finish.zip?dl=0)


## 1. กำหนด URL ใหม่

เปิดไฟล์ `redux/actions.js`

กำหนดค่า endpoint url เป็น URL ของ web api

```js
const endPointURL = 'https://node-web-api-branch.azurewebsites.net/';
```


## 2. เขียน code ส่ง request ไปที่ Web API เพื่อ login


```js
const startSignIn = async (dispatch, navigation, username, password) => {

    // สร้าง object เพื่อแปลงเป็น JSON ส่งไปกับ request
    const signInInfo = {
        username: username,
        password: password
    };

    // ส่ง request ไป Web API เพื่อ login
    const response = await fetch(`${endPointURL}api/auth/signin`, {
        method: 'POST',
        headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
        },
        // แปลง object เป็น JSON
        body: JSON.stringify(signInInfo)
    })

    console.log('Response:', response);

    // ถ้า response status เป็น 200 ถือว่ามีข้อมูลกลับมา
    if (response && response.status === 200) {

        // ถอดข้อความ JSON ออกมาใช้
        const json = await response.json();
        // ดึงค่า token ที่ server ส่งมา 
        console.log('Token:', json.token);
        // เอาเข้า action เพื่อส่งเข้า redux
        dispatch({
            type: Types.SIGN_IN_SUCCESS,
            payload: json.token
        })

        console.log('going to home page')
        navigation.navigate('Home');

    } else {
        console.log(response.status, response.message);
    }
}
```