
# Redux Normalizer

### 1. Set up a new React Native project
Create a new React Native project with expo using the following command:

```bash
npx create-expo-app ReduxToolkitNormalizrProject
```
Navigate to the project directory:

```bash
cd ReduxToolkitNormalizrProject
```

Run command to install redux toolkit and normalizr

```bash
npm install redux react-redux @reduxjs/toolkit normalizr
```

### 2. Define your data schema with Normalizr

Normalizr is a powerful tool for organizing data in your store in a more efficient manner. Start by defining your schema.

```js
// ./schemas.js

import { schema } from 'normalizr';

const user = new schema.Entity('users');
const comment = new schema.Entity('comments', {
  commenter: user
});

const article = new schema.Entity('articles', {
  author: user,
  comments: [comment]
});

export const schemas = {
  "article": article,
  "user": user,
  "comment": comment
};
```

### 3. Create a Redux Slice using Redux Toolkit

Redux Toolkit simplifies the Redux workflow. Create a slice for your articles.

Create a new file `articlesSlice.js` and add the following code:

```javascript
// articlesSlice.js

import { createSlice } from '@reduxjs/toolkit';

const articlesSlice = createSlice({
  name: 'articles',
  initialState: {},
  reducers: {
    addArticle: (state, action) => {
      state[action.payload.id] = action.payload;
    },
  },
});

export const { addArticle } = articlesSlice.actions;

export default articlesSlice.reducer;
```

## 4. Normalize your data

When you receive data, use Normalizr to normalize it before dispatching it to the store.

Create a new file `normalizeData.js` and add the following code:

```javascript
// normalizeData.js
import { normalize } from 'normalizr';
import { schemas } from './schemas';
import { addArticle } from './articlesSlice';
import { useDispatch } from 'react-redux';

export default function NormalizeDataLoad() {

  const dispatch = useDispatch();

  const apiResponse = {
    id: '123',
    author: {
      id: '1',
      name: 'Paul'
    },
    title: 'My awesome blog post',
    comments: [
      {
        id: '324',
        commenter: {
          id: '2',
          name: 'Nicole'
        }
      }
    ]
  };

  const normalizedData = normalize(apiResponse, schemas.article);
  dispatch(addArticle(normalizedData));

  return (
    <>
    </>
  );
}
```


## 5. Create a HomeScreen

Create a new file `HomeScreen.js` and add the following code:

```javascript
// HomeScreen.js

// HomeScreen.js

import React from 'react';
import { View, Text } from 'react-native';
import { useSelector } from 'react-redux';

const HomeScreen = () => {
  const article = useSelector(state => state.articles['123']);


  if (!article) {
    return <Text>No article found</Text>;
  }
  
  console.log('rendering article')
  console.log(article)

  
};

export default HomeScreen;
```


## 6. setup the Redux store

Create a new file `store.js` and add the following code:

```javascript
import { configureStore } from '@reduxjs/toolkit';
import articlesReducer from './articlesSlice';

const store = configureStore({
  reducer: {
    articles: articlesReducer
  }
});

export default store;
```

## 7. App Entry Point

Update the App.js file to display the HomeScreen.

```javascript

import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View } from 'react-native';
import HomeScreen from './HomeScreen';
// import provider from redux
import { Provider } from 'react-redux';
import store from './stores';
import NormalizeDataLoad from './normalizeData';

export default function App() {
  return (
    <Provider store={store}>
      <View>
        <NormalizeDataLoad/>
        <HomeScreen/>
      </View>
    </Provider>
  );
}

```

## 8. Run the application 

Run the application using the following command:

```bash
npm run android
npm run ios
npm run web
```

## 9. Review with redux devtools

Open the Redux DevTools to see the normalized data in the store.

### noted for running on web

```
npx expo install react-native-web react-dom @expo/metro-runtime
```

