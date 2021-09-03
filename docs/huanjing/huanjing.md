# 区分环境

## (1)安装依赖

```javascript

yarn add cross-env -D

```

## (2)在根目录下新建几个文件

- 根目录下创建两个文件，.umirc.test.ts 和 .umirc.pro.ts，内容大致如下

```javascript
// .umirc.test.ts
import { defineConfig } from 'umi';

export default defineConfig({
  define: {
    ENV: 'test',
    BASE_URL: '测试'
  }
})

// .umirc.pro.ts
import { defineConfig } from 'umi';

export default defineConfig({
  define: {
    ENV: 'pro',
    BASE_URL: '生产'
  }
})

```

## (3) 修改 package.json 里面的脚本

- 在 package.json 下面的 script 里面写

```bash
 "dev": "cross-env UMI_ENV=dev umi dev",
 "pro": "cross-env UMI_ENV=pro umi dev",
 "testall": "cross-env UMI_ENV=test umi dev",
```

## (4) 使用的时候敲击不同的命令

- 使用{BASE_URL}

```bash
import styles from './index.less';

export default function IndexPage() {
  return (
    <div>
      <h1 className={styles.title}>Page index {BASE_URL}</h1>
    </div>
  );
}

```
