
# อัพเดตส่วนของ User Interface ด้วย State และ React Hook


## 1. สร้างไฟล์ `CounterView.js`

สร้างไฟล์ `CounterView.js` ตามด้านล่าง และเอามาวางใน `App.js`

```jsx
// CounterView.js

import React from 'react'
import { View, Text, Button } from 'react-native'

export default function CounterView() {

    let count = 0

    const increase = () => {
        count++
        console.log('count:', count)
    }

    return (
        <View>
            <Text>{count}</Text>
            <Button title="เพิ่ม" onPress={increase}></Button>
        </View>
    )
}

```

- ทดสอบกดปุ่ม จะเห็นว่า ค่าตัวแปร count ถูกบวกเพิ่มขึ้นทุกครั้งที่กดปุ่ม แต่หน้าแอพยังเป็นเลข 0 อยู่
- เป็นเพราะว่า โดยปกติ Component จะ render เพียงครั้งเดียวเท่านั้น
- การควบคุมให้ Component render ตัวเองใหม่ จะใช้แนวคิดที่เรียกว่า **state**


## 2. import react hook `useState`

react hook เป็น module หนึ่งที่ติดมากับ react 16.8 เป็นต้นมา เราสามารถนำ react hook ที่ต้องการมาใช้ได้ผ่านคำสั่ง import 

```js
import { useState } from 'react';
```

## 3. ใช้ useState สร้างตัวแปร State

useState สามารถใช้สร้าง **ตัวแปร** และ **function** ที่ใช้อัพเดตตัวแปรนั้น การกำหนดค่าให้ตัวแปรผ่าน function ดังกล่าว เทียบเท่าการใช้คำสั่ง `setState()`

รูปแบบการใช้งาน react hook **useState** จะเป็นดังนี้ 

```
const [ชื่อตัวแปร, function ที่ใช้อัพเดตตัวแปร] = useState(ค่าเริ่มต้นของตัวแปร);
```


## 4. เรียกใช้งาน function เพื่ออัพเดตค่าในตัวแปร state

ดังนั้นหากต้องการ อัพเดตค่าให้ตัวแปร state เราจะทำผ่าน function ที่สร้างขึ้นมาโดยเฉพาะ

เช่น

```js
export default function CounterView() {

    // ในที่นี้เราจะประกาศ ตัวแปร state ขึ้นมา นั่นคือ `counter` และกำหนดค่าเริ่มต้นเป็น 0
    const [counter, setCounter] = useState(0)

    // ไม่ใช้แล้ว เราจะใช้ตัวแปร state ที่ชื่อ counter แทน
    // let count = 0

    const increase = () => {

        // เราจะอัพเดตค่าของตัวแปร state ที่ชื่อ `counter` ด้วย `setCounter()` function ที่ถูกสร้างขึ้นมา
        setCounter(counter + 1)
        console.log('count:', counter)
    }

    return (
        <View>
          {/* นำตัวแปร state มาใช้ได้เหมือนตัวแปรทั่วไป*/}
            <Text>{counter}</Text>
            <Button title="เพิ่ม" onPress={increase}></Button>
        </View>
    )
}
```


# ไฟล์เต็ม `CounterView.js`

```jsx


import React, { useState } from 'react'
import { View, Text, Button } from 'react-native'

export default function CounterView() {

    const [counter, setCounter] = useState(0)
    

    const increase = () => {
        setCounter(counter + 1)
        console.log('count:', counter)
    }

    return (
        <View>
            <Text>{counter}</Text>
            <Button title="เพิ่ม" onPress={increase}></Button>
        </View>
    )
}

```

