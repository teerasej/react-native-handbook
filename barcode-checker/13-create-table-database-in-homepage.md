
# 14. สั่งสร้าง table ใน database เมื่อหน้า App component ถูกโหลดขึ้นมา

เปิดไฟล์​ `App.js` 

```js
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View, SafeAreaView } from 'react-native';
import { NativeBaseProvider, Box, IconButton, Icon, Center } from "native-base";
import HomePage from './pages/home-page/HomePage';
import { FontAwesome } from '@expo/vector-icons';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import ScanPage from './pages/scan-page/ScanPage';

import { Provider } from 'react-redux';
import store from "./redux/store";

// import initDB function
import { initDB } from './services/db.service';

const Stack = createNativeStackNavigator();

// สร้าง log ก่อนการ render App component 
console.log('starting up... ')

export default function App() {

  // เรียกใช้ initDB hook และเราจะได้ตัวแปร isDBReady มาใช้งาน
  const { isDBReady } = initDB();

  // ข้อมูลของ isDBReady
  console.log(`Rendering... [isDBReady: ${isDBReady}]`);

  return (
    <Provider store={store}>
      <NativeBaseProvider>
        <NavigationContainer>
          {  
            // เช็คค่า isDBReady ถ้าไม่ใช่ undefined หรือ false หมายความว่าการ initDB ไม่มีปัญหา และแสดงหน้าแอพปกติ
            isDBReady == true && (
              <Stack.Navigator>
                <Stack.Screen name="Home"
                  component={HomePage}
                  options={({ navigation }) => ({
                    title: 'My home',
                    headerRight: () => (
                      <IconButton
                        icon={<Icon as={FontAwesome} name="qrcode" />}
                        borderRadius="full"
                        onPress={() => navigation.navigate('Scan')}
                      />
                    )
                  })}
                />

                <Stack.Screen name="Scan" component={ScanPage} />

              </Stack.Navigator>

            ) 
          }
          {
            // ถ้าค่า isDBReady เป็น undefined หมายความว่ากำลังอยู่ในขั้นตอนการ init database
            isDBReady == undefined && (
              <Center flex={1}>
                <Box>
                  <Text textAlign="center">Loading</Text>
                </Box>
              </Center>
            )
          }
          {
            // ถ้าค่า isDBReady เป็น false หมายความว่าการ initDB มีปัญหา 
            isDBReady == false && (
              <Center flex={1}>
                <Box>
                  <Text textAlign="center">Error init database</Text>
                </Box>
              </Center>
            )
          }

          <StatusBar style="auto" />
        </NavigationContainer>
      </NativeBaseProvider>
    </Provider>
  );
}

```
