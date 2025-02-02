
# อัพเดตส่วนของ User Interface ด้วย State และ React Hook

สามารถดาวน์โหลดไฟล์โปรเจค [มาจาก Github repository ที่นี่](https://github.com/teerasej/temp-counterapp/tree/master) เพื่อนำมาใช้งานต่อได้


## 1. สร้างไฟล์ `CounterView.tsx`

สร้างไฟล์ `app/CounterView.tsx` ตามด้านล่าง และเอามาวางใน `app/index.tsx`

```jsx


// app/CounterView.tsx
import React from 'react';
import { Box } from '@/components/ui/box';
import { Button, ButtonText } from '@/components/ui/button';
import { Text } from '@/components/ui/text';


export default function CounterView() {
    let count = 0;

    const increase = () => {
        count++;
        console.log('count:', count);
    };

    return <Box>
        <Text>{count}</Text>  
        <Button action={"primary"} variant={"solid"} size={"lg"} >
            <ButtonText>
                เพิ่ม
            </ButtonText>
        </Button>
    </Box>;
}
```

```jsx
// app/index.tsx

import React from "react";
import { View, Alert } from "react-native";
import Hello from "./Hello";

// import CounterView จากไฟล์ CounterView.tsx
import CounterView from "./CounterView";

// ใช้ VStack ในการเรียง Layout
import { VStack } from "@/components/ui/vstack";

export default function Index() {
  
  return <View style={{
    flex: 1,
    justifyContent: "center",
    alignItems: "center"
  }}>
    {/* ใช้ VStack ในการเรียง component แนวดิ่ง */}
    <VStack space="md">
      <Hello name="Nextflow" />

      {/* เรียกใช้ Component CounterView */}
      <CounterView />
    </VStack>
  </View>;
}
```

- ทดสอบกดปุ่ม จะเห็นว่า ค่าตัวแปร count ถูกบวกเพิ่มขึ้นทุกครั้งที่กดปุ่ม แต่หน้าแอพยังเป็นเลข 0 อยู่
- เป็นเพราะว่า โดยปกติ Component จะ render เพียงครั้งเดียวเท่านั้น
- การควบคุมให้ Component render ตัวเองใหม่ จะใช้แนวคิดที่เรียกว่า **state**


## 2. ใช้งาน React Hook `useState` สร้าง State

- react hook เป็น module หนึ่งที่ติดมากับ react 16.8 เป็นต้นมา เราสามารถนำ react hook ที่ต้องการมาใช้ได้ผ่านคำสั่ง import 

1. นำ useState มาใช้งานในไฟล์ `CounterView.tsx`

```jsx
// app/CounterView.tsx

import { Box } from '@/components/ui/box';
import { Button, ButtonText } from '@/components/ui/button';
import { Text } from '@/components/ui/text';

// นำ useState มาใช้งาน
import React, { useState } from 'react';

export default function CounterView() {

    // สร้างตัวแปร state ที่ชื่อ count และกำหนดค่าเริ่มต้นเป็น 0
    const [count, setCount] = useState(0);

    const increase = () => {
        // อัพเดตค่าของตัวแปร state ที่ชื่อ count ด้วย function setCount
        setCount(count + 1);
        // แสดงค่า count ใน console
        console.log('count:', count + 1);
    };

    return (
        <Box>
            <Text>{count}</Text>  
            <Button action={"primary"} variant={"solid"} size={"lg"} onPress={increase}>
                <ButtonText>
                    เพิ่ม
                </ButtonText>
            </Button>
        </Box>
    );
}
```

2. บันทึกไฟล์และทดสอบกดปุ่ม จะเห็นว่าค่า count ถูกบวกเพิ่มขึ้นทุกครั้งที่กดปุ่ม


# ไฟล์เต็ม `app/CounterView.tsx`

```tsx
// app/CounterView.tsx

import { Box } from '@/components/ui/box';
import { Button, ButtonText } from '@/components/ui/button';
import { Text } from '@/components/ui/text';
import React, { useState } from 'react';

export default function CounterView() {
    const [count, setCount] = useState(0);

    const increase = () => {
        setCount(count + 1);
        console.log('count:', count + 1);
    };

    return (
        <Box>
            <Text>{count}</Text>  
            <Button action={"primary"} variant={"solid"} size={"lg"} onPress={increase}>
                <ButtonText>
                    เพิ่ม
                </ButtonText>
            </Button>
        </Box>
    );
}
```

