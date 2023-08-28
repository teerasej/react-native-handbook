
# 10. แสดงข้อความใน Chat History component

## 1. ใช้ useSelector เพื่อดึงข้อมูลจาก slice's state มาใช้กับ FlatList

```jsx
// components/ChatHistory.js

import { View, Text } from 'react-native'
import React from 'react'
import { FlatList, HStack } from 'native-base'
import { useSelector } from 'react-redux'

const ChatHistory = () => {

  const chatHistory = useSelector(state => state.chatroom.chatHistory)

  console.log('Showing chat history:');
  console.log(chatHistory);

  return (
    <>
      {/* กำหนด chatHistory เป็น data ของ FlatList */}
      <FlatList data={chatHistory}
        // กำหนด renderItem function ที่จะ return component ออกไปใน flat list
        // item แต่ละตัวใน data property จะถูกส่งผ่านเข้ามาใน function นี้
        renderItem={({ item }) => (
          <HStack p={3} space={3} flexWrap={'wrap'} >
            <Text>{item.sender}</Text>
            <Text>{item.text}</Text>
          </HStack>
        )}
      >

      </FlatList>
    </>
  )
}

export default ChatHistory
```

## 2. ควบคุมให้ FlatList เลื่อนลงมาล่างสุดเสมอ

```jsx
import { View, Text } from 'react-native'

// import useRef เพื่อสร้างตัวควบคุม flatList
import React, { useRef } from 'react'
import { FlatList, HStack } from 'native-base'
import { useSelector } from 'react-redux'

const ChatHistory = () => {

  const chatHistory = useSelector(state => state.chatroom.chatHistory)

  // สร้าง ref เพื่อเอาไปโยงกับ FlatList
  const flatListRef = useRef();

  console.log('Showing chat history:');
  console.log(chatHistory);

  return (
    <>
      <FlatList data={chatHistory}
        renderItem={({ item }) => (
          <HStack p={3} space={3} flexWrap={'wrap'} >
            <Text>{item.sender}</Text>
            <Text>{item.text}</Text>
          </HStack>
        )}

        // กำหนด ref ให้ component
        ref={flatListRef}
        // เมื่อข้อมูล data มีการเปลี่ยนแปลง เราสั่ง flatList เลื่อนไปล่างสุด ผ่าน ref ที่กำหนด
        onContentSizeChange={() => flatListRef.current.scrollToEnd({ animated: true })}
      >

      </FlatList>
    </>
  )
}

export default ChatHistory
```

## 3. กันปัญหาเรื่อง error: Invariant Violation

อาจจะเจอปัญหา `error: Invariant Violation: Tried to get frame for out of range index -1` เพราะเราพยายามเลื่อน List ไปด้านล่างในขณะที่ยัง render ไม่เสร็จ

```jsx
import { View, Text } from 'react-native'

// import useState เพื่อสร้างตัวควบคุม flatList
import React, { useRef, useState } from 'react'
import { FlatList, HStack } from 'native-base'
import { useSelector } from 'react-redux'

const ChatHistory = () => {

  const chatHistory = useSelector(state => state.chatroom.chatHistory)
  const flatListRef = useRef();

  // สร้าง state เพื่อใช้เป็นกลไกยืนยันการ scroll ไปด้านล่างของ List
  const [isListReady, setIsListReady] = useState(false)

  console.log('Showing chat history:');
  console.log(chatHistory);

  return (
    <>
      <FlatList data={chatHistory}
        renderItem={({ item }) => (
          <HStack p={3} space={3} flexWrap={'wrap'} >
            <Text>{item.sender}</Text>
            <Text>{item.text}</Text>
          </HStack>
        )}

        ref={flatListRef}
        onContentSizeChange={() => {
          // เช็คว่า FlatList render เสร็จแล้ว ค่อยสั่ง scroll ไปที่ท้าย list
          if(isListReady) {
            flatListRef.current.scrollToEnd({ animated: true })
          }
        }}

        // event ท่ี่จะทำงานเมื่อ FlatList สร้าง Layout เสร็จ เราเอามาใช้ในการตั้งค่า เพื่อพิจารณาการเลื่อน List ไปด้านล่างสุด
        onLayout={() => setIsListReady(true)}
      >
      </FlatList>
    </>
  )
}

export default ChatHistory
```