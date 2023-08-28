
# Setup Redux

## 1. สร้าง Reducer 

```jsx
// src/redux/reducer.js


const initialState = {
    count: 0
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case '':
        return { ...state }

    default:
        return state
    }
}
```

## 2. Setup Reducer ให้กับ Redux Store

```jsx
// src/redux/store.js
import { createStore } from "redux";
import reducer from "./reducer";

export default function configureStore() {
    const store = createStore(reducer);
    return store;
}
```