
# นำข้อมูลมาแสดงในหน้า Barcode Page

## 1. ปรับ reducer slice ของเราให้รองรับข้อมูลที่จะได้จากการ Scan Barcode

เราจะต้องปรับ reducer slice ของเราให้รองรับข้อมูลที่จะได้จากการ Scan Barcode โดยเพิ่ม state ใหม่ใน slice ของเรา

```tsx
import { createSlice, PayloadAction } from '@reduxjs/toolkit';
import { askAI } from './askAIThunk';

interface Message {
    text: string;
    isSender: boolean;
}

interface ChatState {
    chatHistory: Message[];

    // เพิ่ม barcode เป็น property ใหม่ใน slice ของเรา
    barcode: string;
}

const initialState: ChatState = {
    chatHistory: [
        { text: 'Hello!', isSender: true },
        { text: 'Hi there!', isSender: false },
        { text: 'How are you?', isSender: true },
        { text: 'I am good, thanks!', isSender: false },
    ],

    // กำหนดค่าเริ่มต้นของ barcode ให้เป็น string ว่าง
    barcode: ''
};

const chatSlice = createSlice({
    name: 'chatSlice',
    initialState,
    reducers: {
        addNewMessageToChatHistory: (state: ChatState, action: PayloadAction<Message>) => {
            console.log(`adding message to history: [${action.type}] ${action.payload}`);
            state.chatHistory.push(action.payload);
        },

        // เพิ่ม reducer สำหรับบันทึก barcode ที่ได้จากการ Scan
        // สังเกตว่า payload ที่ส่งเข้าไปใน reducer นี้คือ string ที่เป็น barcode ที่ได้จากการ Scan
        saveBarcode: (state: ChatState, action: PayloadAction<string>) => {
            console.log(`saving barcode: ${action.payload}`);
            state.barcode = action.payload;
        }
    },
    extraReducers: (builder) => {
        builder.addCase(askAI.fulfilled, (state, action: PayloadAction<string>) => {
            console.log(`adding AI response to history: [${action.type}] ${action.payload}`);
            state.chatHistory.push({ text: action.payload, isSender: false });
        });
        builder.addCase(askAI.rejected, (state, action) => {
            console.error(`AI request failed: [${action.type}] ${action.error.message}`);
            state.chatHistory.push({ text: 'AI request failed. Please try again.', isSender: false });
        });
    }
});

// เพิ่ม reducer function `saveBarcode` ใน slice ของเราลงไปใน action เพื่อไปใช้ใน component อื่น
export const { addNewMessageToChatHistory, saveBarcode } = chatSlice.actions;

export default chatSlice.reducer;
```

## 2. ส่งข้อมูล Barcode ที่ได้จากการ Scan ไปยัง Redux Store

เราจะต้องส่งข้อมูล Barcode ที่ได้จากการ Scan ไปยัง Redux Store โดยใช้ action `saveBarcode` ที่เราได้สร้างไว้ใน reducer slice ของเรา

```tsx

import { Box } from '@/components/ui/box';
import { Text } from '@/components/ui/text';
import React from 'react';
import { BarcodeScanningResult, CameraView, useCameraPermissions } from 'expo-camera';
import { Button, ButtonText } from '@/components/ui/button';

// เรียกใช้ useRouter จาก expo-router เพื่อใช้เปลี่ยนหน้าแอพ กลับไปหน้าก่อนหน้า
import { router, Stack, useRouter } from 'expo-router';

// เรียกใช้ useDispatch เพื่อ dispatch action ไปยัง Redux Store
import { useDispatch } from 'react-redux';

// เรียกใช้ AppDispatch จาก store ของเรา เพื่อใช้ในการ dispatch action
import { AppDispatch } from '@/redux/store';

// เรียกใช้ action ที่เราสร้างไว้ใน slice ของเรา
import { saveBarcode } from '@/redux/chatSlice';

export default function ScanPage() {

    const [permission, requestPermission] = useCameraPermissions();

    // เรียกใช้ dispatch จาก react-redux เพื่อเรียกใช้ action ใน Redux Store
    const dispatch:AppDispatch = useDispatch()
    // เรียกใช้ router จาก expo-router เพื่อเปลี่ยนหน้าแอพ
    const router = useRouter()

    const handleBarcodeScanned = (result:BarcodeScanningResult) => {
        console.log(result.data)

        // เรียกใช้ action `saveBarcode` ที่เราสร้างไว้ใน slice ของเรา
        dispatch(saveBarcode(result.data))
        // เมื่อ Scan เสร็จแล้วให้กลับไปหน้าก่อนหน้า
        router.back()
    }

    if (!permission) {
        return (
            <Box className="flex-1 justify-center align-middle">
                <Text>Requesting for camera permission...</Text>
            </Box>
        )
    }

    if (permission.granted === false) {
        return (
            <Box className="flex-1 justify-center align-middle">
                <Text>We need your permission to show the camera</Text>
                <Button onPress={requestPermission}>
                    <ButtonText>Request permission</ButtonText>
                </Button>
            </Box>
        )
    }

    return (
        <Box className='flex-1'>
            <Stack.Screen options={{
                title: "Scan"
            }} />

            <CameraView 
                style={{ flex: 1 }}
                barcodeScannerSettings={{
                   barcodeTypes:['code128','ean13','ean8','qr']
                }}
                onBarcodeScanned={handleBarcodeScanned}
                >

            </CameraView>
        </Box>
    );
}
```

## 3. แสดงข้อมูล Barcode ที่ได้จากการ Scan ในหน้า Barcode Page

เราจะต้องแสดงข้อมูล Barcode ที่ได้จากการ Scan ในหน้า Barcode Page โดยใช้ข้อมูลที่เราได้จาก Redux Store

```tsx
// app/barcode/index.tsx

import { Button, ButtonText, ButtonSpinner, ButtonIcon } from '@/components/ui/button';

import React from 'react';
import { Stack, useRouter } from 'expo-router';
import { Box } from '@/components/ui/box';
import { Text } from '@/components/ui/text';

// เรียกใช้ useSelector เพื่อดึงข้อมูล barcode ที่เราเก็บไว้ใน Redux Store
import { useSelector } from 'react-redux';

// เรียกใช้ rootState จาก store ของเรา เพื่อกำหนดเป็น type ของ state ที่ได้จาก redux store
import { RootState } from '@/redux/store';


export default function index() {

  const router = useRouter()

  // เรียกใช้ useSelector เพื่อดึงข้อมูล barcode ที่เราเก็บไว้ใน Redux Store
  const barcode = useSelector((state:RootState) => state.chatroom.barcode)

  const handleStartScan = () => {
    router.push('/barcode/scan')
  }
  
  // แสดงข้อมูล barcode ที่ได้จากการ Scan โดยใช้ condition ว่าถ้ามีข้อมูล barcode ให้แสดงข้อมูลนั้น ถ้าไม่มีให้แสดงข้อความว่า "No barcode scanned"
  return <>
    <Stack.Screen options={{
      title: "Barcode Scanner"
    }}/>
    
      <Box className="flex-1 p-2">
        <Button onPress={handleStartScan}>
          <ButtonText>Scan</ButtonText>
        </Button>
        { 
          barcode.length > 0 ?
          <Text>{barcode}</Text> 
          : <Text>No barcode scanned.</Text>
        }
      </Box>
  </>;
}