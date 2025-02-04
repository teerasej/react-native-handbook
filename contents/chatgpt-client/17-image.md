
# การใช้งานรูปภาพ

เราสามารถใช้งานรูปภาพบนแอพ ผ่าน Image Component ของ React Native ได้ โดยการใช้งาน Image Component นั้น จะมีการรับ prop ที่สำคัญดังนี้

```tsx
// app/index.tsx

import React from "react";
import { Link } from "expo-router";
import { VStack } from "@/components/ui/vstack";
import { Stack } from "expo-router";
import { Button, ButtonText } from "@/components/ui/button";

// เรียกใช้ Image Component จาก gluestack
import { Image } from "@/components/ui/image";

export default function Home() {

  // 1. การใช้งาน Image Component แบบที่หนึ่งคือการเรียกผ่าน require โดยใช้ path ของไฟล์รูปภาพใน assets ของโปรเจค
  // 2. การใช้งาน Image Component แบบที่สองคือการเรียกผ่าน uri ที่กำหนดที่อยู่ของ URL ของรูปภาพจาก public internet
  return (
    <>
      <Stack.Screen
        options={{
          title: "Home"
        }}
      />

      <VStack className="flex-1 p-2" space="md">

        <Image source={require('assets/images/favicon.png')} size="none" />
        <Image source={{uri:'https://github.com/teerasej/react-native-gluestack-typescript-my-chatgpt/blob/eas-build/assets/images/thaiairways.png?raw=true'}} alt="logo"/>

        <Link href="/chat" asChild>
          <Button>
        <ButtonText>Ask AI</ButtonText>
          </Button>
        </Link>

        <Link href="/barcode" asChild>
          <Button>
        <ButtonText>Scan Barcode</ButtonText>
          </Button>
        </Link>
      </VStack>

    </>
  );
}

```

## Reference 

- เนื่องจาก Image ของ Gluestack ต่อยอดจาก Image ของ React native โดยตรง ลองไป[เช็คการใช้งานที่นี่ได้](https://reactnative.dev/docs/image)