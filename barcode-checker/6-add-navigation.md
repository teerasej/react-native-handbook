
# 6. เพิ่มระบบ Navigation

## 1. กำหนดข้อมูลให้หน้า Home Page

เปิดไฟล์ `pages/home-page/HomePage.js`

เราจะเพิ่ม property `navigationOptions` เข้าไปใน **HomePage** เพื่อใช้สำหรับระบบ React Navigation

```js

export class HomePage extends Component {

    static navigationOptions = {
        // กำหนด title ของส่วนนี้เป็น Home
        title: 'Home'
    };

    //...
}
```

## 2. สร้างระบบ Navigation 

เปิดไฟล์ `App.js`

```js
import { createAppContainer } from 'react-navigation';
import { createStackNavigator } from 'react-navigation-stack';

const AppNavigator = createStackNavigator({
  Home: { screen: HomePage }
});

const AppContainer = createAppContainer(AppNavigator);
```

**AppContainer** คือระบบที่เราสามารถเอาไปใช้กับ Redux Provider ได้ 

## 3. ถ้ามีการเอา AppContainer ไปใช้ตอนนี้... จะ error 

ถ้าใช้ `AppContainer` ใส่ลงไปใน `<HomePage>` แทนแบบนี้

```jsx
render() {
    if (!this.state.isReady) {
      return <AppLoading />;
    }

    return (
        <AppContainer />
    );
  }
```

ดังนั้นตอนนี้ถ้าเจอ Error ก็ไม่ใช่เรื่องผิดปกติอะไร เพราะปกติ **AppContainer** ต้องเอาไปใช้กับ Redux Provider นั่นเอง
