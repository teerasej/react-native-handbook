
# Create a modular React Native application

### 1. Set up a new React Native project
Create a new React Native project with expo using the following command:

```bash
npx create-expo-app MyModuleProject
```
Navigate to the project directory:

```bash
cd MyModuleProject
```

### 2. Create a new module

Create a new module called `modules` using the following command:

```bash
mkdir modules
```

### 3. Create a new module directory

Create a new directory called `UserModule` inside the `modules` directory:

```bash
mkdir modules/UserModule
```

Create a new file called `UserView.js` and `UserModel.js` inside the `UserModule` directory:

```bash
touch modules/UserModule/UserView.js
touch modules/UserModule/UserModel.js
```

## 4. Implement the UserView component

Add the following code to the `UserView.js` file:

```javascript
import React from 'react';
import { View, Text } from 'react-native';

const UserView = ({ user }) => (
  <View>
    <Text>{user.name}</Text>
    <Text>{user.email}</Text>
  </View>
);

export default UserView;
```

## 5. Implement the UserModel component

Add the following code to the `UserModel.js` file:

```javascript
class UserModel {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
}

export default UserModel;
```

## 6. Use the UserView and UserModel in your App

Update the `App.js` file to use the `UserView` and `UserModel` components:

```javascript
import UserView from './modules/UserModule/UserView';
import UserModel from './modules/UserModule/UserModel';
```

Create a new UserModel and render it using UserView:

```javascript
const user = new UserModel('John Doe', 'john.doe@example.com');

const App = () => (
  <View>
    <UserView user={user} />
  </View>
);

export default App;
```

## 7. Run the application 

Run the application using the following command:

```bash
npm run android
npm run ios
npm run web
```

### noted for running on web

```
npx expo install react-native-web react-dom @expo/metro-runtime
```