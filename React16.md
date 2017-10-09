# React16

## 新特性
1. **render 函数可返回数组和字符串。**
>不必增加嵌套了，数组需带有 key属性。
>
>>- 在 React 15 中，一个组件的 render() 方法是不能返回一个数组作为多个根元素的，如果有这种需求的话，可能需要在这些元素外额外包裹一层`<div>`。
>这样做不仅会产生多余的 DOM 元素，还可能因为 DOM 结构变化导致 CSS 布局出现问题。
>
>>		class Select extends React.Component{
>>			render() {
>>				return(
>>					<div>
>>						<Button />
>>						<Popover />
>>					</div>
>>				); 
>>			}
>>		} 
>
>>- 在 React 16 中，新增加了一个特性叫 fragments，允许组件的 render() 方法返回一个 fragment（数组）来同时渲染多个根元素。
>
>>		class Select extends React.Component {
>>			render() {
>>				return [
>>					<Button key="button" />,
>>					<Popover key="popover" />,
>>				];
>>			}
>>		}
>>
>>>对于 fragments 这个新特性，讨论比较多的一点是是否支持在使用了 fragment 的组件上调用 **ReactDOM.findDOMNode(...) 。**
>
>>>目前没有看到任何明确的结论表示支持或者不支持。使用 React 16 最新版（16.0.0-beta.5）测试得到的结果是**ReactDOM.findDOMNode(...)** 会返回 **render()** 返回的 fragment 中第一个元素对应的 DOM 元素。
>
>>>在 jsx 这边有一个新的语法提案，意在简化 fragments 的使用方式：
>>>
>>>		class Select extends React.Component {
>>>			render() {
>>>				return (
>>>					<>
>>>						<Button />
>>>						<Popover />
>>>					</>
>>>				);
>>>			}
>>>		}
  

1. **更好的错误处理。**
>现在你可以在组件内部出错时使用一个替代的 UI。
>
>新的生命周期方法 - componentDidCatch, 它能捕获在子组件树中任何地方的 JavaScript 异常，并打印这些错误和展示备用UI, 就像将 children 包裹在一个大的 try/catch 语句块中.
>
>那么, 错误处理究竟可以干些什么
>
>- 当有错误发生时, 我们可以友好地展示 fallback 组件
>- 	避免 normal 和 fallback 组件耦合
>- 	我们可以清楚地发现某些服务的一些报错
>- 	我们可以复用报错和 fallback

1. **Portal 函数。**
>可以在 DOM任意位置插入内容。现在想弹个对话框或 tooltip更方便了。

1. **超快的 SSR 。**
>React 16改进了服务端渲染，据测试，相比 React 15.6.1，在不同版本的 Node环境下可达到 5-10倍的性能提升。

1. **支持自定义的 DOM 属性。**
>现在不用使用属性 whitelist了，React会将未知属性直接传到 DOM中。

1. **减小了库文件大小。**
>现在的 React核心库大小仅 5.3kb（gzip后 2.2kb），相比原来缩减了 30%。

1. **更改为 MIT开源协议。**
>上周 Facebook发表博客表示怂了，从 React 16开始更改为更宽松的 MIT协议。对于某些不方便升级的 React用户，还发布了 15.6.2版本，也改用了 MIT协议。

---
除了上面这些之外，React 16变化最大的是因为使用了**新引擎 React Fiber**。

Facebook正在以流行的JavaScript框架React为基础开发一个全新的架构。这个名为**React Fiber**的全新设计改变了检测变更的方法和时机，借此可改进浏览器端和其他渲染设备的响应速度。

这一全新架构最初已于2016年7月公开发布，其中蕴含着过去多年来Facebook不断改进的工作成果。**该架构可向后兼容，彻底重写了React的协调（Reconciliation）算法。**该过程可用于确定出现变更的具体时间，并将变更传递给渲染器。

实际上该团队在单线程JavaScript引擎的基础上构建了一种可划分优先级的协作式任务调度器。**在最初的协调算法（现已更名为Stack Reconciler）**中，Virtual DOM diff会一次性处理整个组件树：

>重点在于，Stack Reconciler始终会一次性地同步处理整个组件树。Stack Reconciler无法暂停，因此如果更新较为深入并且可用CPU时间有限，这种做法并非最优化的。

与之相对的**Fiber Reconciler**则有着截然不同的目标：

>1. 能够将可中断的任务拆分成块。
>
>2. 能够对进程中的工作划分优先级、重新设定基址（Rebase）、恢复。
>
>3. 能够在父子之间来回反复，借此为React的Layout提供支持。
>
>4. 能够通过render()返回多个元素。
>
>5. 为错误边界提供了更好的支持。

简单来说，此时不在需要等待变更传播到整个组件树，**React Fiber**可以知道如何时不时暂停一下，检查是否有其他更重要的更新。这种调度能力主要基于**requestIdleCallback**的使用，而这是一种W3C候选推荐标准。
