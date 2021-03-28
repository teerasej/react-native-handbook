
# สร้างหน้า New Note Page

## 1. สร้างไฟล์ NewNotePage.js

```jsx
// pages/new-note-page/NewNotePage.js
// snippet: rnf
import React from 'react'
import { View, Text } from 'react-native'

export default function NewNotePage() {
    return (
        <View>
            <Text>New Note Page</Text>
        </View>
    )
}
```

เสร็จแล้วเอาไปแทนที่ `<HomePage/>` ในไฟล์ `App.js` เป็นการชั่วคราว

```jsx
// App.js

export default function App() {
    
    //...
    
    return (
        <NewNotePage/>
    )
}
```



## 2. สร้างส่วนของ UI ด้วย Nativebase

```jsx
// pages/new-note-page/NewNotePage.js

import { Body, Header, Button, Container, Content, Input, Item, Label, Title } from 'native-base'

import React from 'react'
import { View, Text } from 'react-native'

export default function NewNotePage() {

    return (
        <Container>
            <Header>
                <Body>
                    <Title>New Note</Title>
                </Body>
            </Header>
            <Content padder>
                {/* สร้าง Label และ Field */}
                <Item stackedLabel>
                    <Label>Message: </Label>
                    <Input placeholder="Message" />
                </Item>
                {/* ปุ่มยืนยันการสร้าง Note */}
                <Button block primary>
                    <Text>Save</Text>
                </Button>
            </Content>
        </Container>
    )
}
```

