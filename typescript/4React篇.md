# React篇

本篇将介绍如何在 react 中规范的使用 ts 进行开发，因为有很长一段时间没有用过类组件了，所有这里只介绍函数式组件的写法。（估计不会补充了类组件的内容了）

#### 内置的类型

首先我们来了解一下 react 内置的 ts 类型。<br />

##### React.CSSProperties

如果我们需要向组件传递 style 样式参数，可以通过这个内置对象去定义。

```
interface IProps {
    style?: CSSProperties | undefined;
}
```

#### 定义组件

```
type React.FC<T = {}> = React.FunctionComponent<T>  // 函数式组件的类型

interface IProps {
  name: string
}

const App: React.FC<IProps> = (props) => {
  const {name, children} = props;
  return (
    <div className="App">
      <div>{name}</div>
      {props.children}
    </div>
  );
}

export default App;
```
通过这个方式定义的组件会帮我们默认地在 props 上带上 children 属性。我们无需在接口 IProps 中定义，就可以在使用它。因为 React.FC 这个类型默认的给我们定义了函数的返回值应该是一个 ReactElement, 给函数添加了静态属性 displayName、propTypes、defaultProps。 同时也给传入的 props 添加了 children 属性，默认是 ReactElement | null。


#### useState

```
const [name, setName] = useState<string | null>(null); 

interface IProps {
    setName: React.Dispatch<React.SetStateAction<string>
}
```
值得注意的是，如果当我们需要将 setName 这个方法传入到子组件的时候，我们最好按照上面 IProps 这个接口这样去定义 setName 这个方法的属性，会使它更加精准。

#### useRef

```

```