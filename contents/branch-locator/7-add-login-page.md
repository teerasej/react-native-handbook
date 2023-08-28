
# 7. เพิ่ม Login Page

## 1. ตั้ง login page เป็นหน้าแรก

เปิดไฟล์ `App.js`

และเปิดตัว options `initialRouteName` 

```js
const AppNavigator = createStackNavigator(
  {
    Home: { screen: HomePage },
    Login: { screen: LoginPage },
  },
  {
    initialRouteName: "Login"
  }
);
```

## 2. ซ่อนปุ่มย้อนกลับในหน้า Home

เปิดไฟล์ `pages/home-page/HomePage.js`

ตั้งค่า `headerLeft` เป็น `null` 

```js
static navigationOptions = ({ navigation }) => {
        return {
            title: 'Home',
            headerLeft: null
        };
    };
```