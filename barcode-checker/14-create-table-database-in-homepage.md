
# 14. สั่งสร้าง table ใน database เมื่อหน้า App component ถูกโหลดขึ้นมา

เปิดไฟล์​ `App.js` 

```js
import DBService from './services/db.service';

export default class App extends React.Component {

    async componentDidMount() {
        await Font.loadAsync({
        Roboto: require('native-base/Fonts/Roboto.ttf'),
        Roboto_medium: require('native-base/Fonts/Roboto_medium.ttf'),
        ...Ionicons.font,
        });

        // สั่ง init database
        await new DBService().init();

        this.setState({ isReady: true });
    }


}
```