# การเข้าถึงค่า Props ใน Function component 

- ค่าที่ถูกส่งเข้ามาผ่าน Attribute จะสามารถเข้าถึงได้ ในรูปแบบของ object ตัวหนึ่งที่ชื่อว่า `props` 
- ชื่อของ attribute จะกลายเป็นค่า property ของ object เช่นถ้าส่งเป็น `<Hello name="พล">` ก็จะเป็น `props.name` จากในใน component


เปิดไฟล์ **Hello.js** และปรับโค้ดตามด้านล่าง

```jsx
const Hello = (props) => {
    return (
        <View>
            <Text style={styles.helloText}>Hello {props.name}</Text>
        </View>
    )
}
```

ทดสอบใช้งาน

## De-construct object props

- เราสามารถประกาศชื่อให้กับค่าที่ส่งเข้ามาโดยตรง และไม่ต้องเรียกผ่าน `props` หรือ `this.props` ได้ด้วย 
- ผลจากการทำวิธีนี้ เราสามารถควบคุมค่าที่ส่งเข้ามาใน Component ได้โดยตรง ไม่ dynamic เมื่อการเรียกใช้ค่าผ่าน `props`
- เช่น ถ้าเราส่งค่าเข้ามา `<Hello name="พล" website="nextflow">` ถ้าเราต้องการสร้างตัวแปรจากค่าที่ส่งเข้ามาทาง props สามารถเขียนได้แบบนี้

### แบบ Function component 

```jsx
// <Hello name="พล" website="nextflow">

// Deconstruct object props โดยตรง
const Hello = ({ name, website}) => {
    return (
        <View>
            {/* นำชื่อตัวแปรที่ได้จากการ deconstruct มาใช้ */}
            <Text style={styles.helloText}>Hello {name} from {website}</Text>
        </View>
    )
}
```