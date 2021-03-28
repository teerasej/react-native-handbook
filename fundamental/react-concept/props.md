
# Props

- การส่งค่าเข้ามาใน `props` ของ component จะดูเหมือน HTML attribute ตามปกติ
- ค่า HTML attribute จะเป็นชื่อของ property ใน `props` เช่น `<Nextflow logo="brand.jpg">` ก็จะเป็น `props.logo` 

## การส่งค่า props

เราสามารถกำหนดชื่อ และค่าของ attribute ลงไปใน JSX ได้เลย

```jsx
<Hello name="พล"/>
```

และสามารถแทรกค่าตัวแปรลงไปใน JSX ได้ผ่านเครื่องหมาย expression หรือ `{}`

```jsx
let username = 'nextflow';

<Goodbye name={username}/>
```

> สังเกตว่า ถ้าเราแทรกค่าตัวแปรเข้าไปใน attribute เราไม่จำเป็นต้องใส่เครื่องหมาย `""`

## การเข้าถึงค่า Props ในการเขียนแบบต่างๆ 

- [แบบ Function Component](props-function.md)
- [แบบ Class Component](props-class.md)
  
