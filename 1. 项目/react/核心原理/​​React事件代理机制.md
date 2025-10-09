React 的事件代理机制是其高效处理用户交互的核心设计，通过​**​事件委托（Event Delegation）​**​和​**​合成事件（Synthetic Event）​**​实现

**事件委托（Event Delegation）​**

- React 不会将事件监听器直接绑定到每个 DOM 元素，而是委托到​**​根节点​**​
- 当子元素触发事件（如点击），事件通过 DOM 冒泡/捕获传递到根节点，React 根据事件目标（`event.target`）定位对应的组件实例并调用其事件处理函
- 组件卸载时自动清理|

**合成事件（Synthetic Event）**

- 对原生事件（如 `MouseEvent`）标准化处理，提供统一 API（如 `e.preventDefault()`、`e.stopPropagation()`），消除浏览器差异


优势

- ：统一不同浏览器的事件行为
- 顶层委托减少内存占用
- 自动绑定/解绑