## 增加 dva models

在 umi 里面

符合下面的规则的文件都会被认为是 model 文件

- `src/models` 下的文件

- `src/pages` 下,子目录中的 models 目录下的文件

- `src/pages` 下,所有 model.ts 文件(不分区大小写)

例如

```bash

+ src
  + models/a.ts
  + pages
    + foo/models/b.ts
    + bar/model.ts

```

### 使用

- 新建文件夹 models 下面的 Home.ts (Home.ts 名字随便起)

里面代码

```javascript
import { Effect, Subscription, Reducer } from 'umi';
import servicedata from '../service/getdata';
export interface StateType {
  result: string;
}

export interface HomeModel {
  namespace: 'Home';
  state: StateType;
  reducers: {
    show: Reducer,
  };
  effects: {
    getdata: Effect,
  };
  subscriptions: {
    setup: Subscription,
  };
}

const Homedata: HomeModel = {
  namespace: 'Home',
  state: {
    result: '',
  },
  reducers: {
    show(state, { payload }) {
      console.log(payload);
      const stateresult = JSON.parse(JSON.stringify(state));
      stateresult.result = payload;
      console.log(stateresult);
      return stateresult;
    },
  },
  effects: {
    *getdata({ payload }, { put, call }) {
      const result = yield call(servicedata);
      console.log(result.data.list[0].name);
      yield put({
        type: 'show',
        payload: result.data.list[0].name,
      });
    },
  },
  subscriptions: {
    setup({ dispatch, history }) {
      return history.listen((location) => {
        if (location.pathname === '/home/page1') {
          dispatch({
            type: 'show',
            payload: '要显示的公共数据',
          });
        }
      });
    },
  },
};

export default Homedata;
```

- 对应的 services 文件

```javascript
import axios from 'axios';
let data = { code: '1234', name: 'yyyy' };

const getdata = () => {
  return axios.post('http://localhost:8000/api/student', data).then((res) => {
    console.log(res);
    return res;
  });
};

export default getdata;
```

- 页面里面使用的时候注意 type 必须和 namespace 对应

```javascript
import * as React from 'react';
import { history, connect, ConnectProps } from 'umi';
import { StateType } from '@/models/Home';
interface IPageOneProps extends ConnectProps {
  value: string;
  handleChange: () => void;
}

const PageOne: React.FunctionComponent<IPageOneProps> = (props) => {
  console.log(props);
  function gotourl() {
    history.push({
      pathname: '/home/page2',
      state: {
        a: '传递的值',
      },
    });
  }
  return (
    <div>
      <span>第一页</span>
      <button onClick={gotourl}>跳转</button>
      <div>{props.value}</div>
      <button onClick={props.handleChange}>点击改变</button>
    </div>
  );
};

const mapState = (state: any) => {
  console.log(state);
  return {
    value: state.Home.result,
  };
};

const mapMutations = (dispatch: any) => {
  return {
    handleChange() {
      dispatch({
        type: 'Home/getdata',
      });
    },
  };
};
export default connect(mapState, mapMutations)(PageOne);
```
