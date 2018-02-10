# Redux

리덕스를 학습하고 기록합니다.

모든 문서는 다음 경로를 참고하여 만들었습니다. 
https://velopert.com/3346

## Reducer

>액션의 `type`에 따라 변화를 일으키는 함수.

#### initialState

초기상태(initialState)가 Reducer에 정의됩니다.

```js
// src/reducers/index.js
import * as types from '../actions/ActionTypes';

const initialState = {
    color: 'black',
    number: 0
};
```

#### function

리듀서는 state와 action을 파라미터로 받습니다. action.type에 따라 다른 작업을 하고, 새 상태를 만들어서 반환합니다. 기존 상태 값에 원하는 값을 덮어쓴 새로운 객체를 만들서 반환해야 합니다.

```js
// src/reducers/index.js
function counter(state = initialState, action) {
    switch (action.type) {
        case types.INCREMENT: 
            return {
                ...state,
                number: state.number + 1
            };
        case types.DECREMENT:
            return {
                ...state,
                number: state.number - 1
            };
        default:
            return state;
    }
};

export default counter;
```

## Store

>state를 내장하고 있고, subscribe중인 함수들이 업데이트 될 때 마다 다시 실행되게 해준다.

```js
// src/index.js
// Redux 관련 불러오기
import { createStore } from 'redux'
import reducers from './reducers';

// 스토어 생성
const store = createStore(reducers);
```

리듀서를 이용하여 스토어를 생성합니다.

## Connect

> Container에서 presentational component를 App의 store와 연결시킨다.

```js
// src/Container/CounterContainer.js
const CounterContainer = connect(
    mapStateToProps,
    mapDispatchToProps
)(Counter);
```

컴포넌트의 Props을 매핑시킬 상태와, 액션함수를 전달해줍니다.
connect로 반환된 함수 안에 **presentational component**를 파라미터로 전달해주면
store에 연결된 컴포넌트가 만들어집니다.

#### mapStateToProps

store안의 state 값을 props로 가져옵니다.

```js
// CounterContainer.js
const mapStateToProps = (state) => ({
    color: state.color,
    number: state.number
});
```

#### mapDispatchToProps

액션함수를 생성하고 dispatch하는 함수를 생성 후, 이를 props으로 연결해준다.

```js
// CounterContainer.js
const mapDispatchToProps = (dispatch) => ({
    onIncrement: () => dispatch(actions.increment()),
    onDecrement: () => dispatch(actions.decrement()),
    onSetColor: () => {
        const color = 'black'; // 임시
        dispatch(actions.setColor(color));
    }
});
```

