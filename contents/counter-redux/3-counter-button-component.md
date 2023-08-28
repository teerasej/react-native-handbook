
# สร้างส่วนปุ่มเพิ่มจำนวน Counter

สร้างไฟล์ `src/components/IncreaseButton.js`

```jsx
// rnc
// src/components/IncreaseButton.js

import React, { Component } from 'react'
import { View, Text,Button } from 'react-native'
import PropTypes from 'prop-types'

export default class IncreaseButton extends Component {

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

```