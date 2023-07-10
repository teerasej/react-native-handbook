
## ใช้งาน Navigation props 

ทุก Stack.Screen component จะได้รับ `navigation` props เพื่อเรียกใช้งานในการเคลื่อนไปยังส่วนต่างๆ 

```jsx
// page/HomePage.js

import { View, Text } from 'react-native'
import React from 'react'
import { Box, Button, Container, HStack } from 'native-base'

// เรียกใช้ props ชื่อ navigation สิ่งนี้ได้จาก NativeBaseProvider
const HomePage = ({ navigation }) => {
  return (
    <>
      <Container style={{ padding: 10 }}>
        {/* เรียกใช้ navigation ในการเปิดไปยัง Screen ที่ชื่อว่า Chat */}
        <Button onPress={() => navigation.navigate('Chat')}  style={{ marginBottom: 10 }} >
          <Text style={{ color: 'white' }} >Chat</Text>
        </Button>
        {/* เรียกใช้ navigation ในการเปิดไปยัง Screen ที่ชื่อว่า Settings */}
        <Button onPress={() => navigation.navigate('Settings')}>
          <Text style={{ color: 'white' }} >Setting</Text>
        </Button>
      </Container>
    </>
  )
}

export default HomePage
```

เสร็จแล้วทดสอบการใช้งานดู