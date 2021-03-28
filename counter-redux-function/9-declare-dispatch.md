
## ส่ง Action เข้า Redux ด้วย `useDispatch()` hook

```jsx
// IncreaseButton.js 

import React from 'react'
import { Button, StyleSheet, Text, View } from 'react-native'

// เรียกใช้ useDispatch hook
import { useDispatch } from 'react-redux'

// เรียกใช้ action ที่สร้างไว้ เพื่อกำหนดชื่อ type ให้ object ที่จะส่งเข้า redux
import actions from './redux/actions'

export default function IncreaseButton() {

    // เรียก function ชื่อ dispatch 
    const dispatch = useDispatch()

    const increase = () => {
        console.log('increase...')
        
        // ใช้ function dispatch ในการส่ง Action object เข้า Redux 
        // จะเห็นว่า เรากำหนด type ด้วยชื่อ action ที่สร้างไว้ และ payload คือข้อมูลที่ต้องการส่งไปกับ action ให้ reducer
        dispatch({
            type: actions.ADD,
            payload: 1
        })
    }

    return (
        <View>
            <Button title="เพิ่ม" onPress={increase}></Button>
        </View>
    )
}

const styles = StyleSheet.create({})

```