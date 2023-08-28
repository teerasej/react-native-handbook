
# เรียกใช้ redux state ใน component

## 1. กำหนดค่าเริ่มต้นของ Redux state 

```jsx
// redux/reducer.js

// กำหนดค่าให้กับ redux state object 
// ค่านี้จะถูกส่งไป component ทุกตัวที่ต่อกับ redux ผ่าน hook ที่ชื่อ useSelector() ตอนเริ่มแอพพลิเคชั่น
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

## 2. เรียกใช้งาน Redux state ผ่าน `useSelector()` 


```jsx

import React from 'react'
import { StyleSheet, Text, View } from 'react-native'
import { useSelector } from 'react-redux'  
export default function CounterNumber() {

    // function ใน useSelector จะได้รับ redux state ในตอนเริ่ม render ครั้งแรก และทุกครั้งที่ reducer return ค่า redux state object ใหม่ออกมา จะทำให้ component render ตัวเองใหม่

    // เราจึงสามารถเลือกค่า property จาก redux state มาเก็บในตัวแปร เพื่อใช้ใน component ได้ 
    
    // ตัวอย่างนี้ เราดึง property .count 
    const count = useSelector(state => state.count)

    return (
        <View>
            {/* สามารถเรียกใช้ตัวแปร ที่ได้จาก useSector  */}
            <Text style={styles.counter}>{count}</Text>
        </View>
    )
}

const styles = StyleSheet.create({
    counter: {
        fontSize: 60
    }
})

```