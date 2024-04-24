
# Create a modular React Native application

### 1. Set up a new React Native project
Create a new React Native project with expo using the following command:

```bash
npx create-expo-app CleanArchitectureApp
```
Navigate to the project directory:

```bash
cd CleanArchitectureApp
```

### 2. Create the directory structure

Create a directory structure that separates concerns. Here's a simple example:

```
/
├── components/
├── screens/
├── services/
├── models/
└── utils/
```

Create the directories using the following command:

```bash
mkdir components screens services models utils
```

### 3. Components

Create a Button component in the components directory. This component should receive a title and onPress props.

```javascript
// components/Button.js

import React from 'react';
import { Button as RNButton } from 'react-native';

const Button = ({ title, onPress }) => (
  <RNButton title={title} onPress={onPress} />
);

export default Button;
```

## 4. Services

Create a UserService in the services directory. This service should have a getUser method that returns a user object.

```javascript
// services/UserService.js

const getUser = () => {
  return { id: 1, name: 'John Doe' };
};

export default { getUser };
```

## 5. Screens

Create a HomeScreen in the screens directory. This screen should use the UserService to get a user and display the user's name. It should also use the Button component.



```javascript
// screens/HomeScreen.js

import React, { useEffect, useState } from 'react';
import { Text, View } from 'react-native';
import Button from '../components/Button';
import UserService from '../services/UserService';

const HomeScreen = () => {
  const [user, setUser] = useState(null);

  useEffect(() => {
    const user = UserService.getUser();
    setUser(user);
  }, []);

  return (
    <View>
      {user && <Text>{user.name}</Text>}
      <Button title="Click me" onPress={() => alert('Clicked!')} />
    </View>
  );
};

export default HomeScreen;
```

## 6. App Entry Point

Update the App.js file to display the HomeScreen.

```javascript
// App.js

import React from 'react';
import HomeScreen from './screens/HomeScreen';

const App = () => {
  return <HomeScreen />;
};

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

## 8. Add Use case

Create a use case to get the user from the UserService and display it on the HomeScreen.

```javascript
// useCases/GetUserUseCase.js

import UserService from '../services/UserService';

const execute = () => {
  return UserService.getUser();
};

export default { execute };
```

## 9. Update the HomeScreen

Update the HomeScreen to use the GetUserUseCase.

```javascript
// screens/HomeScreen.js

import React, { useEffect, useState } from 'react';
import { Text, View } from 'react-native';
import Button from '../components/Button';
import GetUserUseCase from '../usecases/GetUserUseCase';

const HomeScreen = () => {
  const [user, setUser] = useState(null);

  useEffect(() => {
    const user = GetUserUseCase.execute();
    setUser(user);
  }, []);

  return (
    <View>
      {user && <Text>{user.name}</Text>}
      <Button title="Click me" onPress={() => alert('Clicked!')} />
    </View>
  );
};

export default HomeScreen;
```

## 10. Run the application 

Run the application using the following command:

```bash
npm run android
npm run ios
npm run web
```

This exercise should give you a basic understanding of how to implement the use case pattern in a clean architecture project. The key is to encapsulate the business logic in use cases, which are then called by the screens (or other parts of your code). This makes your code more modular, easier to test, and easier to understand.

 
