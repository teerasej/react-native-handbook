
# สร้างส่วนแสดงจำนวน Counter

สร้างไฟล์ `src\components\CounterNumber.js`

```jsx
// snippet: rnc
// src/components/CounterNumber.js

import React, { Component } from 'react'
import { View, Text } from 'react-native'
import PropTypes from 'prop-types'

export default class CounterNumber extends Component {
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
```