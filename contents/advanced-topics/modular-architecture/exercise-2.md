
# Using the module 

You have to finish the [1st exercise](exercise-1.md) before starting this one.

This way, other parts of your application can import from UserModule without needing to know the internal structure of the module.

## 1. Create the module's index file

Create a new file called `index.js` inside the `UserModule` directory:

```bash
touch modules/UserModule/index.js
```

## 2. implement the module's index file

Add the following code to the `index.js` file:

```javascript
import UserView from './UserView';
import UserModel from './UserModel';

export { UserView, UserModel };
```

## 3. Import the module in the App.js file

Add the following code to the `App.js` file:

```javascript
import React from 'react';
import { View } from 'react-native';

// Notice that we are importing the module from the index file
import { UserView, UserModel } from './modules/UserModule';

...
```

### Full `App.js` code 

```javascript
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View } from 'react-native';
import { UserView, UserModel } from './modules/UserModule';

export default function App() {

  const user = new UserModel('John Doe', 'john.doe@example.com');
  
  return (
    <View style={styles.container}>
      <Text>Open up App.js to start working on your app!</Text>
      <UserView user={user} />
      <StatusBar style="auto" />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
});
```


