
# React Navigation & Redux

## React Navigation

1. [เตรียมโปรเจคให้พร้อมใช้งาน](1-setup.md)
2. [สร้างระบบ Navigation](2-setup-navigation.md)

## Redux

> ดูเพิ่มเติม [Redux for React](https://redux.js.org/basics/usage-with-react)

Redux ประกอบไปด้วย 3 ส่วนที่ต้องสร้างขึ้นมา เพื่อให้ทำงานสอดประสานกันเป็นหนึ่งเดียว เหมือนทีมฟุตบอล หรือทีมเกมส์​ MOBA ต้องมีทั้งรุก รับ support มีฝ่ายใดฝ่ายหนึ่งไม่ได้ 

3 ฝ่ายหลักคือ 
- กลุ่ม Reducer
- กลุ่ม Store
- กลุ่ม Actions (type และ action object)

## Redux Reducer และ Redux Store

3. [สร้าง Redux Reducer](3-setup-redux-reducer.md)
4. [สร้าง Redux Store](4-setup-redux-store.md)
5. [Setup redux store เข้ากับ navigation ผ่าน Provider](5-setup-redux-store-to-provider.md)
6. [กำหนดปุ่ม Save ให้มีการย้อนกลับไปหน้าแรก](6-save-button-to-home.md)
7. [เชื่อม HomePage และ NewNotePage เข้า Redux](7-connect-to-redux.md)
8. [เรียกใช้ค่าจาก redux state ใน HomePage](8-access-redux-state.md)

## Redux Action 

9. [สร้าง Action ที่เกิดขึ้นในระบบ](9-action.md)
10. [กำหนด function ที่ต้องการสร้าง Action ใน NewNotePage](10-map-dispatch-action.md)
11. [กำหนด Action Type ที่ Reducer ต้องเอาข้อมูลมาอัพเดตใน redux state](11-reducer-update-state.md)
