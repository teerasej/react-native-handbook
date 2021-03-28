
# ใช้ redux state จาก reducer ใน component

## 1. กำหนดค่าเริ่มต้นใน Redux state

```jsx
// src/redux/reducer.js

// กำหนดค่าให้กับ redux state object เริ่มต้น ค่านี้จะถูกส่งไป component ทุกตัวที่ต่อกับ redux store ผ่าน mapStateToProps()
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

## 2. map ค่าจาก Redux state object ให้กับ props object เพื่อเอาไปใช้ใน component


```jsx
import React, { Component } from 'react'
import { connect } from 'react-redux'

export class CounterNumber extends Component {
    static propTypes = {
        count: PropTypes.number
    }

    static defaultProps = {
        count: 0
    }

    render() {
        return (
            <View>
                <Text>{this.props.count}</Text>
            </View>
        )
    }
}

const mapStateToProps = (state) => ({
    // กำหนด ค่าจาก state ที่ต้องการ เข้ากับ property ของ props object
    count: state.count
})

const mapDispatchToProps = {
    
}

export default connect(mapStateToProps, mapDispatchToProps)(CounterNumber)
```