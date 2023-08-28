
# 3. Test action types 

สร้างไฟล์​ `redux/actions.type.test.js`

```js

import actions from "./actions";

describe('Actions Type', () => {
    
    it('Exist', () => {
        const actionType = actions.Types;
        expect(actionType).toBeDefined();
        expect(actionType.GET_BRANCH_LIST_SUCCESS).toBeDefined();
        expect(actionType.LOCATION_DEVICE_FOUND).toBeDefined();
        expect(actionType.SIGN_IN_SUCCESS).toBeDefined();
    });

});
```