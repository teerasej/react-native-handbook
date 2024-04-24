
# Use Context API in React


## 1. Set up a new React Native project
Create a new React Native project with expo using the following command:

```bash
npx create-expo-app ContextAPIExample
```
Navigate to the project directory:

```bash
cd ContextAPIExample
```

## 2. Create a Context

In your project directory, create a new file called `ThemeContext.js` This file will hold our Context object.

```javascript
// ThemeContext.js

import React from 'react';

const ThemeContext = React.createContext();

export default ThemeContext;
```

## 3. Provide the Context

Now, we need to provide the context to the components that need it. Open your `App.js` file and import the ThemeContext.

```javascript
// App.js

import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View } from 'react-native';
import ThemeContext from './ThemeContext';

export default function App() {
  return (
    <ThemeContext.Provider value={{ theme: 'light' }}>
      <View style={styles.container}>
        <Text>Open up App.js to start working on your app!</Text>
        <StatusBar style="auto" />
      </View>
    </ThemeContext.Provider>
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


## 4.  Consume the Context

Finally, we can consume the context in any child component. Create a new file called `ThemedText.js`.

```javascript
// ThemedText.js

import React, { useContext } from 'react';
import { Text } from 'react-native';
import ThemeContext from './ThemeContext';

export default function ThemedText() {
  const { theme } = useContext(ThemeContext);

  return <Text style={{ color: theme === 'light' ? '#000' : '#fff' }}>This is a themed text.</Text>;
}
```

## 5. Don't forget to import and use ThemedText in your App.js.


```javascript
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View } from 'react-native';
import ThemeContext from './ThemeContext';

export default function App() {
  return (
    <ThemeContext.Provider value={{ theme: 'light' }}>
      <View style={styles.container}>
        <Text>Open up App.js to start working on your app!</Text>
        <ThemedText />
        <StatusBar style="auto" />
      </View>
    </ThemeContext.Provider>
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


## 6. Run the application 

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

### Challange 

Try to make the theme dynamic by adding a button that changes the theme from light to dark and vice versa.


## 8. Update your context value dynamically

To update the context value dynamically in a React Native application, you can use a state variable in your context provider and provide a function to update this state. 

### 1. Here's an example based on your ThemeContext

```javascript
// ThemeContext.js

import React, { useState } from 'react';

const ThemeContext = React.createContext();

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');

  const toggleTheme = () => {
    setTheme((currentTheme) => (currentTheme === 'light' ? 'dark' : 'light'));
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

export default ThemeContext;
```

### 2. Update your App.js to use the ThemeProvider

```javascript
// App.js

import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View } from 'react-native';
import ThemedText from './ThemeText';
import { ThemeProvider } from './ThemeContext';

export default function App() {
  return (
    <ThemeProvider>
      <View style={styles.container}>
        <Text>Open up App.js to start working on your app!</Text>
        <ThemedText/>
        <StatusBar style="auto" />
      </View>
    </ThemeProvider>
  );
}
```

### 3. Update your ThemedText.js to use the toggleTheme function

```javascript
// ThemedText.js

import React, { useContext } from 'react';
import { Text, Button,View } from 'react-native';
import ThemeContext from './ThemeContext';

export default function ThemedText() {
  const { theme, toggleTheme } = useContext(ThemeContext);

  return (
    <View>
      <Text style={{ color: theme === 'light' ? '#000' : '#fff' }}>This is a themed text.</Text>
      <Button title="Toggle Theme" onPress={toggleTheme} />
    </View>
  );
}
```

### 4. Run the application 

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