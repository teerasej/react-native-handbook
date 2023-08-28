
# เขียน Test Unit ให้กับ note.reducer.js

### Setup Test Environment

- [ทำการติดตั้ง Test Environment](test/1-setup.md)


สร้างไฟล​์​ `redux/reducers/note.reducer.test.js`

```js

import reducer from "./note.reducer";
import actions from "./../actions";

describe('Note Reducer', () => {
    
    it('Initial Note should be empty', () => {
        expect(reducer(undefined, {}).notes).toEqual([]);
    });

    it('Notes should have new item', () => {
        const action = {
            type: actions.ActionTypes.SAVE_NEW_NOTE,
            payload: 'hello'
        }

        const notes = reducer(undefined, action).notes;

        expect(notes.length).toBe(1);
        expect(notes[0]).toStrictEqual({ title: 'hello'});
    });

    it('Notes should have 2 new item', () => {
        const action1 = {
            type: actions.ActionTypes.SAVE_NEW_NOTE,
            payload: 'hello'
        }

        const action2 = {
            type: actions.ActionTypes.SAVE_NEW_NOTE,
            payload: 'test'
        }

        const state = reducer(undefined, action1);
        const finalState = reducer(state, action2);

        expect(finalState.notes.length).toBe(2);
        expect(finalState.notes[0]).toStrictEqual({ title: 'hello'});
        expect(finalState.notes[1]).toStrictEqual({ title: 'test'});
    });

});
```