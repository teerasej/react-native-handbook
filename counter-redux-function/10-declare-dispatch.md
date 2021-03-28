

```jsx
import React, { Component } from 'react'
import { View, Text,Button } from 'react-native'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'
import action from '../redux/action'


export class IncreaseButton extends Component {

    addMoreNumber = () => {
        // เรียกใช้ dispatch function ผ่าน props object
        this.props.add(1);
    }


    render() {
        return (
            <div>
                
                <button onClick={this.addMoreNumber}>เพิ่ม</button>
            </div>
        )
    }
}

const mapStateToProps = (state) => ({
    
})

const mapDispatchToProps = (dispatch) => {
    // สร้าง object ที่มี function เป็น property
    // object นี้จะสามารถเข้าถึงได้ จากภายใน component ผ่าน this.props
    return {
        add: (amount) => dispatch({ type: action.Actions.ADD_NUMBER, payload: amount })
    }
}

export default connect(mapStateToProps, mapDispatchToProps)(IncreaseButton)
```