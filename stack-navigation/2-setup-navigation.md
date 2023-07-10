
# 2. Setup Stack Navigation 

เราจะทำการเรียกใช้ function และ component ที่ `@react-navigation/native` เตรียมไว้ในการ setup แผนที่ของแอพเราขึ้นมา

```jsx
// App.js
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View } from 'react-native';
import { NativeBaseProvider, Icon } from "native-base";
import { FontAwesome } from '@expo/vector-icons';
import HomePage from './pages/HomePage'
import ChatPage from './pages/ChatPage'
import SettingPage from './pages/SettingPage'


// import NavigationContainer 
import { NavigationContainer } from '@react-navigation/native';
// import function createBottomTabNavigator ในการสร้าง Tab component
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';

// สร้าง Tab component 
const Tab = createBottomTabNavigator();

export default function App() {
  return (
    <NativeBaseProvider>
      {/* setup  NavigationContainer */}
      <NavigationContainer>
        {/* สร้าง Tab Navigator  */}
        <Tab.Navigator>

          {/* กำหนด Screen Home และ component HomePage */}
          <Tab.Screen
            name="Home" 
            component={HomePage} 
            // กำหนด Tab bar icon
            options={{
              tabBarIcon: ({ color, size }) => (
                <Icon as={FontAwesome} name="home" color={color} size={size} />
              )
            }} 
          />
          {/* กำหนด Screen Chat และ component ChatPage */}
          <Tab.Screen
            name="Chat" 
            component={ChatPage} 
            // กำหนด Tab bar icon
            options={{
              tabBarIcon: ({ color, size }) => (
                <Icon as={FontAwesome} name="comment" color={color} size={size} />
              )
            }} 
          />
          {/* กำหนด Screen Setting และ component SettingPage */}
          <Tab.Screen 
            name="Setting" 
            component={SettingPage} 
            // กำหนด Tab bar icon
            options={{
              tabBarIcon: ({ color, size }) => (
                <Icon as={FontAwesome} name="gear" color={color} size={size} />
              )
            }} 
          />
        </Tab.Navigator>
        <StatusBar style="auto" />
      </NavigationContainer>
    </NativeBaseProvider>
  );
}

```