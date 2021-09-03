# 全局布局

## 新建页面文件

### 新建 Home 文件

- 在 src 目录下找到 pages 新建文件夹 Home,然后新建文件 Home.tsx

- 里面写入代码

```javascript
import * as React from 'react';

import { Layout, Menu } from 'antd';
const { Header, Footer, Sider } = Layout;
import ContentComponent from '../../components/Home/Content.tsx';
interface ILayOutProps {}
import styleshome from './home.less';
const LayOut: React.FunctionComponent<ILayOutProps> = (props) => {
  return (
    <Layout>
      <Header>
        <div className={styleshome.logo}>测试 </div>
        <Menu
          theme="dark"
          mode="horizontal"
          defaultSelectedKeys={['1']}
          style={{ lineHeight: '64px' }}
        >
          <Menu.Item key="1">列表1</Menu.Item>
          <Menu.Item key="2">列表2</Menu.Item>
          <Menu.Item key="3">列表3</Menu.Item>
        </Menu>
      </Header>
      <ContentComponent></ContentComponent>
      <Footer style={{ textAlign: 'center' }}>
        Umi 入门教程 Created by xiaohuoni
      </Footer>
    </Layout>
  );
};

export default LayOut;
```

### 同级目录下新建 Component 文件

```javascript
import * as React from 'react';
import { Layout } from 'antd';
const { Content } = Layout;
interface IContentProps {}

const ContentComponent: React.FunctionComponent<IContentProps> = (props) => {
  return (
    <div>
      <Content style={{ padding: '0 50px' }}>
        <span>主体内容</span>
      </Content>
    </div>
  );
};

export default ContentComponent;
```

### 这里特别说明下 less 文件

- 新建 home.less 文件 里面写

```css
.logo {
  width: 148px;
  color: white;
  margin: 16px 24px 16px 0;
  float: left;
  font-size: 18px;
  line-height: 30px;
}
```

- 使用的时候

```javascript
import styleshome from './home.less';

<div className={styleshome.logo}>测试 </div>;
```
