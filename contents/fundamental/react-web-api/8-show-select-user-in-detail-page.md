
# 8. แสดงรายละเอียด User ที่ถูกเลือกใน Detail Page

เปิดไฟล์ `pages/detail-page/DetailPage.js`

## 1. เรียกใช้ค่า selectedUser มาจาก redux state

```js
export default function DetailPage() {

    // เลือกดึง property selectedUser มาจาก redux state
    const selectedUser = useSelector(state => state.selectedUser)
```

## 2. แสดงข้อมูล User ที่ถูกเลือก ใน JSX 

```jsx
return (
        <Content padder>
            <List>
                <ListItem>
                    <Text>ชื่อ: {selectedUser.name.first}</Text>
                </ListItem>
                <ListItem>
                    <Text>นามสกุล: {selectedUser.name.last}</Text>
                </ListItem>
            </List>
            <Button iconLeft block
                onPress={() => callUser(selectedUser.phone)}
                style={{
                    marginTop: 10
                }}>
                <Icon name="call" />
                <Text>โทร: {selectedUser.phone}</Text>
            </Button>
        </Content>
    )
```

## A. ไฟล์เต็ม `pages/detail-page/DetailPage.js`

```jsx
import React from 'react'
import { Content, List, ListItem, Text, Button, Icon } from 'native-base';
import { useSelector } from 'react-redux';

export default function DetailPage() {

    const selectedUser = useSelector(state => state.selectedUser)

    return (
        <Content padder>
            <List>
                <ListItem>
                    <Text>ชื่อ: {selectedUser.name.first}</Text>
                </ListItem>
                <ListItem>
                    <Text>นามสกุล: {selectedUser.name.last}</Text>
                </ListItem>
            </List>
            <Button iconLeft block
                
                style={{
                    marginTop: 10
                }}>
                <Icon name="call" />
                <Text>โทร: {selectedUser.phone}</Text>
            </Button>
        </Content>
    )

}

```