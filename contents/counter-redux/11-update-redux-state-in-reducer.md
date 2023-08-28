
# กำหนดให้ reducer จัดการ Action และ payload 

```js
// import action type
import action from "./action"

const initialState = {
    count: 0
}

export default (state = initialState, { type, payload }) => {
    switch (type) {


    // เทียบชื่อ action 
    case action.Actions.ADD_NUMBER:
        console.log(type, payload)
        // update redux state และส่งให้ component
        return { ...state, count: state.count + payload }

    default:
        return state
    }
}

```