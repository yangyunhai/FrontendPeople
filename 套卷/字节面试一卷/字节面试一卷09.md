## 面试官：使用React hooks如何只让下面这段代码的子组件只render一次？
```js
>代码大致如下，子组件有自己的状态但受父组件的控制，当父组件count更新的时候，需要将子组件的number和父组件的count保持同步，但是使用useEffect和useMemo都会让子组件render两次，有什么方法可以只render一次吗？

import React, { useEffect, useMemo, useState } from 'react'

function A() {
  const [count, setCount] = useState(0)

  return <div>
    <p>我是父组件</p>
    <p>父组件的count是{count}</p>
    <button onClick={() => setCount(count + 1)}>click</button>
    <B count={count} />
  </div>
}

const B = React.memo(({count}: {count: number}) => {
  const [number, setNumber] = useState(0)
  useMemo(() => {
    setNumber(count)
  }, [count])

  console.log('子组件render')
  return <div>
    <p>我是子组件</p>
    <p>子组件的number是{number}</p>
    <button onClick={() => setNumber(number + 1)}>click</button>
  </div>
})

export default A
```


### 解析

```js
import React, { useEffect, useMemo, useState, useRef } from 'react';
function A() {
    const [count, setCount] = useState(0);

    return (
        <div>
            <p>我是父组件</p>
            <p>父组件的count是{count}</p>
            <button onClick={() => setCount(count + 1)}>click</button>
            <B count={count} />
        </div>
    );
}

const B = React.memo(({ count }: { count: number }) => {
    const numberRef = useRef(0);
    const [, update] = useState({});
    const updateNumber = () => {
        numberRef.current++;
        update({});
    };

    useMemo(() => {
        numberRef.current = count;
    }, [count]);

    console.log('子组件render');

    return (
        <div>
            <p>我是子组件</p>
            <p>子组件的number是{numberRef.current}</p>
            <button onClick={updateNumber}>click</button>
        </div>
    );
});
```

使用useRef保存子组件状态，当父组件更新时，直接更新ref值；当子组件click时，在更新ref值后，再调用一次update触发子组件渲染。

#### 在线示例：
>https://codesandbox.io/s/headless-dawn-vvyu9

### 参考资料
* https://www.zhihu.com/people/qiqiboy-81
* https://www.zhihu.com/question/444068787
* https://www.zhihu.com/question/444068787/answer/1739616037