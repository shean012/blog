### react-query

#### 前言
其实在react更新 hook 和 Suspense 标签不久后，我就开始使用 react-query 作为react项目的异步数据管理工具。<br>
我认为要介绍这个库，首先得弄明白，为什么我们需要用它。所以我们先思考几个问题。
1. 在项目中，如果在 useEffect 中请求数据是否存在不妥的地方？
2. 对于请求回来的数据，目前的项目是如何管理的？（页面切换）
3. 对于页面中用户手动触发的数据请求，我们时如何管理按钮状态的？
4. 如果在一个页面中，我们同时需要发起多个请求，来获取不同的业务数据，目前是如何做的？
5. 在 react18 中如何更好的利用并发模式，带来更好的用户体验？
6. 更好的贴合 react hook

#### 介绍
react-query 对自己的评价描述是 Powerful asynchronous state management for TS/JS，React，Solid, Vue ... <br/>
不难看出，react-query其实是一款 状态管理 工具，有别于其他状态管理，它是专门服务于异步数据的，在此之前，我们管理异步状态，通常使用 redux-saga、redux-thunk，对于我个人来说，他们都不怎么好用，尤其是对于新语法不兼容，一个项目里，同时会使用 async/await 、promise 、generator function 三种语法，真的不太友好。redux-thunk 导致 reducer 不再是纯函数，我个人来说，宁愿使用 saga。但 saga 不但体积大，写法麻烦，性能还没有隔壁家 mobx 好。就在这个尴尬的时候，我遇上了 react-query 。便马上喜欢上这个 异步状态管理工具。

#### 状态

react-query 和他的状态管理工具不一样，它是通过 querykey 来管理所有的异步状态，它需要在项目的外层嵌套一个 QueryClient 组件，它就像一个 Provider 一样，所有在这个 QueryClient 里面使用的 querykey 都会被收集，当我们需要获取 异步状态 的时候，只要通过我们刚刚定义的 querykey 去获取就可以的，从语法上来说，这个东西是超级简单易懂的。而更让人舒服的是使用 react-query 的语法，十分贴合 react hook，看看下面的例子就明白了。

```
const { data, isLoading, ... } = useQuery(["querykey"], async() => {})
```
通过例子可以看到当我们需要发起异步请求的时候，我们通过 useQuery 这个自定义 hook 包裹我们的异步函数，为什么我会说语法十分舒服，贴合 react-hook 呢，可以到 useQuery 的第一个参数，是一个数组，里面放着我们的 querykey, 但同时我们可以放入我们需要观察的变量，当数组中的变量发生变化时，会重新执行 useQuery 中的函数，这就和我们的 useEffect 语法十分相似。而在我们请求回来的值，retrun 出去，便可以通过 data 这个参数获取得到。于此同时我们还能得到 isloading 等对于本次请求状态的参数，我们可以通过这个参数，控制页面的按钮等组件的状态。<br>
data 的中的异步数据 react-query 会存入内存中，就如上面说到的 Provider , 我们在页面中需要取回异步状态，语法和 content api 很类似。
```
const queryClient = useQueryClient();
const state = queryClient.getQueryState("queryKey");
```
如果 queryKey 是存在的，我们就能获取到之前请求的数据，否则我们将得到一个 undefined。<br>
多数情况下我们使用 react-query ，就是用 useQuery 这个 hook, 又因为 react-query 用法十分简单，所以我们将更多的笔墨花在介绍用法以外的地方。

#### 回顾

我们回顾


