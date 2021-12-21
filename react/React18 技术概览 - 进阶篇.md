# JSX
## 为什么要使用
React 认为**渲染逻辑**本质上与其他 UI 逻辑内在耦合，比如，在 UI 中需要绑定处理事件、在某些时刻状态发生变化时需要通知到 UI，以及需要在 UI 中展示准备好的数据。

React 并没有采用将*标记与逻辑进行分离到不同文件*这种人为地分离方式，而是通过将二者共同存放在称之为“组件”的松散耦合单元之中，来实现[*关注点分离*](https://en.wikipedia.org/wiki/Separation_of_concerns)。

React [不强制要求](https://react.docschina.org/docs/react-without-jsx.html)使用 JSX，但是大多是的时候，在 JavaScript 代码中将 JSX 和 UI 放在一起时，会在视觉上有辅助作用。它还可以使 React 显示更多有用的错误和警告消息。
## 需要关注什么
-   因为 JSX 语法上更接近 JavaScript 而不是 HTML，所以 React DOM 使用 `camelCase`（**小驼峰命名**）来定义属性的名称，而不使用 HTML 属性名称的命名约定。例如，JSX 里的 `class` 变成了 [`className`](https://developer.mozilla.org/en-US/docs/Web/API/Element/className)，而 `tabindex` 则变为 [`tabIndex`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/tabIndex)。

-   JSX可以防止注入攻击：React DOM 在渲染所有输入内容之前，默认会进行[转义](https://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html)。它可以确保在你的应用中，永远不会注入那些并非自己明确编写的内容。所有的内容在渲染之前都被转换成了字符串。这样可以有效地防止 [XSS（cross-site-scripting, 跨站脚本）](https://en.wikipedia.org/wiki/Cross-site_scripting)攻击。

- 可以使用Babel(@babel/preset-react) 把 JSX 转译成一个名为 `React.createElement()` 函数调用，而不必借助webpack loader等处理器

**下面两种写法完全等价**
```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>)
```


```js
const element = React.createElement(
  'h1', /* type */ 
  {className: 'greeting'}, /* props */
  'Hello, world!'/* children */
);
```
# ReactElement
## 常用字段和XSS
下面截图是上面代码createElement后的产物，本质上就是个React Object（ extends Object）：
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2ebf99f253b34699a26f601e45c15168~tplv-k3u1fbpfcp-zoom-1.image)

- key: 组件的key，主要用在virtual dom上，compare diff和move element
- props：组件属性，来源于父组件或者HOC，或者类似的外部传递
- ref：当前的dom引用
- type: 组件类型
- _owner: 是React Component，创建react component的组件，空值为null
- `$$typeof`: 早期的React（0.13）版本中[很容易](http://danlec.com/blog/xss-via-a-spoofed-react-element)受到 XSS 攻击，为了解决此问题，字段名是使用$$typeof，value使用的是Symbol 

React如何解决此问题，通过两种方式：（参考文档 [why-do-react-elements-have-typeof-property](https://overreacted.io/zh-hans/why-do-react-elements-have-typeof-property/)）

    -   编码时：React等新兴代码库会进行转义（客户端）
    -   数据传输时：由于JSON不支持$$属性名，所以该属性会被过滤掉（服务器端）

## Object vs ReactObject
如何区分object是否是react object？React提供了isValidElement方法
```js
/**
* Verifies the object is a ReactElement.
* See 
https://reactjs.org/docs/react-api.html#isvalidelement

* @param {?object} object
* @return {boolean} True if `object` is a ReactElement.
* @final
*/
export function isValidElement(object) {
    return (
        typeof object === 'object' &&
        object !== null &&
        object.$$typeof === REACT_ELEMENT_TYPE
    );
}
```

# Fiber node
代码中的声明如下
```js
function FiberNode(
  tag: WorkTag,
  pendingProps: mixed,
  key: null | string,
  mode: TypeOfMode,
) {
  // Instance
  // 静态数据存储的属性
  // 定义光纤的类型。在reconciliation算法中使用它来确定需要完成的工作。如前所述，工作取决于React元素的类型。函数createFiberFromTypeAndProps将React元素映射到相应的光纤节点类型
  this.tag = tag;
  this.key = key;
  this.elementType = null;
  // 定义与此光纤关联的功能或类。对于类组件，它指向构造函数，对于DOM元素，它指定HTML标记。我经常使用此字段来了解光纤节点与哪些元素相关。
  this.type = null;
  // 保存对组件，DOM节点或与光纤节点关联的其他React元素类型的类实例的引用。通常，我们可以说此属性用于保存与光纤关联的局部状态。
  this.stateNode = null;

  // Fiber
  // Fiber关系相关属性，用于生成Fiber Tree结构
  this.return = null;
  this.child = null;
  this.sibling = null;
  this.index = 0;

  this.ref = null;

  // 动态数据&状态相关属性
  // new props,新的变动带来的新的props，即nextProps
  this.pendingProps = pendingProps;
  // prev props，用于在上一次渲染期间创建输出的Fiber的props
  this.memoizedProps = null;
  // 状态更新，回调和DOM更新的队列，Fiber对应的组件，所产生的update，都会放在该队列中
  this.updateQueue = null;
  // 当前屏幕UI对应状态，上一次输入更新的Fiber state
  this.memoizedState = null;
  // 一个列表，存储该Fiber依赖的contexts，events
  this.dependencies = null;
  
  // conCurrentMode和strictMode
  // 共存的模式表示这个子树是否默认是 异步渲染的
  // Fiber刚被创建时，会继承父Fiber
  this.mode = mode;

  // Effects
  // 当前Fiber阶段需要进行任务，包括：占位、更新、删除等
  this.flags = NoFlags;
  this.subtreeFlags = NoFlags;
  this.deletions = null;

  // 优先级调度相关属性
  this.lanes = NoLanes;
  this.childLanes = NoLanes;

  // current tree和working in prgoress tree关联属性
  // 在FIber树更新的过程中，每个Fiber都有与其对应的Fiber
  // 我们称之为 current <==> workInProgress
  // 在渲染完成后，会指向对方
  this.alternate = null;

  // 探查器记录的相关时间
  if (enableProfilerTimer) {
    // this.actualDuration 真正渲染时长(毫秒级别)
    // this.actualStartTime 渲染开始时间
    // this.selfBaseDuration
    // this.treeBaseDuration 子树渲染时长
 
    // Note: The following is done to avoid a v8 performance cliff.
  }

  // 调试相关
  if (__DEV__) {
    // This isn't directly used but is handy for debugging internals:
  }
}
```
# 类型转化（重点）
React中的每个组件都有一个UI表示形式，我们可以调用从该`render` 方法返回的视图或模板。下面的是示例代码`ClickCounter` 

```js
<button key="1" onClick={this.onClick}>Update counter</button>
<span key="2">{this.state.count}</span>
```
## JSX到ReactElement
模板通过JSX编译器后，您最终将获得一堆React元素。这实际上是从`render` React组件的方法返回的，而不是HTML。由于我们不需要使用JSX，因此可以像下面这样重写组件的`render` 方法`ClickCounter`
```js
class ClickCounter {
    ...
    render() {
        return [
            React.createElement(
                'button',
                {
                    key: '1',
                    onClick: this.onClick
                },
                'Update counter'
            ),
            React.createElement(
                'span',
                {
                    key: '2'
                },
                this.state.count
            )
        ]
    }}
```
方法`React.createElement`中对的调用`render`将创建两个数据结构，如下所示：
```js
[
    {
        $$typeof: Symbol(react.element),
        type: 'button',
        key: "1",
        props: {
            children: 'Update counter',
            onClick: () => { ... }
        }
    },
    {
        $$typeof: Symbol(react.element),
        type: 'span',
        key: "2",
        props: {
            children: 0
        }
    }]
```
您可以看到React Object中的[`  $$typeof `](https://overreacted.io/why-do-react-elements-have-typeof-property/) 属性，它React Element的唯一地标识。然后`props` 描述的元素属性并传递给`React.createElement`函数。

`ClickCounter`的React元素没有任何props 或key：

```js
{
    $$typeof: Symbol(react.element),
    key: null,
    props: {},
    ref: null,
    type: ClickCounter
}
```
## ReactElement到FiberNodes
在 **reconciliation** 期间，从`render`方法返回的每个React元素的数据都将合并到Fiber tree中。每个ReactElement都有一个对应的Fiber Node用来保存组件状态和DOM的可变数据结构。与React元素不同，并不是在每个渲染器上都重新创建Fiber（可复用）。

React中，框架会根据React元素的类型执行不同的活动。在我们的示例应用中，对于类组件，`ClickCounter`它调用生命周期方法和`render`方法，而对于`span`宿主组件（DOM节点），它执行**DOM mutation**。因此，每个React元素都被转换为[相应类型](https://github.com/facebook/react/blob/769b1f270e1251d9dbdce0fcbd9e92e502d059b8/packages/shared/ReactWorkTags.js)的Fiber节点，该节点描述了需要完成的工作。

**您可以将**Fiber**视为代表要完成的某些工作或换句话说，一个工作单元的数据结构。Fiber的体系结构还提供了一种方便的方式来跟踪，安排，暂停和中止工作。**

首次将React元素转换为Fiber时，React使用元素中的数据在[createFiberFromTypeAndProps](https://github.com/facebook/react/blob/769b1f270e1251d9dbdce0fcbd9e92e502d059b8/packages/react-reconciler/src/ReactFiber.js#L414)函数中创建Fiber。在随后的更新中，React重用了Fiber，并仅使用来自相应React元素的数据来更新必要的属性。`key`相同的React元素不再从`render`方法中返回，React可能还需要根据prop在层次结构中移动或删除节点。

React为每个React元素创建了一个Fiber node，我们通过ReactElement Tree可以生成Fiber Tree。在我们的示例应用程序中，它看起来像这样：

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cadb66ac4d0d4bd1a9d16af735f3c9b3~tplv-k3u1fbpfcp-watermark.image)

所有Fiber nodes的节点都通过链表连接：`child`，`sibling`和`return`。

上一篇内容：
[React18 技术概览 - 基础篇](https://juejin.cn/post/7008131204297785358) https://juejin.cn/post/7008131204297785358


下一篇预告：源码实现

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f5b1da8604ff483ea60a23cc3cb01a83~tplv-k3u1fbpfcp-watermark.image?)
