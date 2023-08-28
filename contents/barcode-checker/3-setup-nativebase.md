
# 3. Setup NativeBase

## เขียน Logic ให้แน่ใจว่าตัว App โหลด font ของ Native base UI พร้อมใช้งานได้แน่นอน

```jsx
// App.js
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View } from 'react-native';
import { NativeBaseProvider, Box } from "native-base";

export default function App() {
  return (
    <NativeBaseProvider>
        <Box>
          Hello
        </Box>
      <StatusBar style="auto" />
    </NativeBaseProvider>
  );
}


```


