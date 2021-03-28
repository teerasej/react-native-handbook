
# เชื่อม Component เข้ากับ Redux store

## AddNumber component

```jsx
// src/components/IncreaseButton.js

import React, { Component } from 'react'
import { View, Text,Button } from 'react-native'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'
import action from '../redux/action'

// ลบ default ออกจาก class เพราะย้ายไปใช้กับ connect() function ด่านล่างสุดแทน
export class IncreaseButton extends Component {

    addMoreNumber = () => {
        
    }


    render() {
        return (
            <div>
                <button onClick={this.addMoreNumber}>เพิ่ม</button>
            </div>
        )
    }
}

// ใช้จับคู่ redux state กับ props ของ component
const mapStateToProps = (state) => ({
    
})

// ใช้จับคู่ function กับ props ของ component
const mapDispatchToProps = () => {
}

// export class ที่เชื่อมต่อ กับ 
export default connect(mapStateToProps, mapDispatchToProps)(IncreaseButton)
```

## Counter component

```jsx
// src/components/CounterNumber.js
import React, { Component } from 'react'
import { View, Text } from 'react-native'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'

// ลบ default ออกจาก class เพราะย้ายไปใช้กับ connect() function ด่านล่างสุดแทน
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
})

const mapDispatchToProps = {
    
}

export default connect(mapStateToProps, mapDispatchToProps)(CounterNumber)

```