## 使用***MutationObserver***来监听dom的增删改查触发事件

> 因为工作时遇到了这么一个需求。项目中的datePicker与dataGrid并不是同一套组件，他们显示的层级在弹出errorMessage的情况下，会导致z-index高度过低，无法正确显示层级。为了解决这个问题，我只好在业务页面监听body节点下所有的顶级节点是否有生成目标元素。如果生成，就调低高度(在有errorMessage的情况下)以下为实现的源码。

```typescript
// Create an observer instance
private Observer = new MutationObserver(function (mutations) {
    mutations.forEach(function (mutation) {
        const newNodes = mutation.addedNodes; // DOM NodeList
        if (newNodes && newNodes.length > 0) {
            for (let i = 0; i < newNodes.length; i++) {
                if (newNodes[i]['className'].indexOf('wj-dropdown') !== -1
                    && newNodes[i]['className'].indexOf('wj-inputdate') !== -1) {
                    // 过滤后业务处理
                }
            }
        }
        const removedNodes = mutation.removedNodes; // DOM NodeList
        if (removedNodes && removedNodes.length > 0
            && removedNodes[0]['className'].indexOf('wj-dropdown') !== -1
            && removedNodes[0]['className'].indexOf('wj-inputdate') !== -1) {
            for (let i = 0; i < removedNodes.length; i++) {
                if (removedNodes[i]['className'].indexOf('wj-dropdown') !== -1
                    && removedNodes[i]['className'].indexOf('wj-inputdate') !== -1) {
                    // 删除节点时再把高度重置
                }
            }
        }
    });
});

private addZindexfixer() {
    // The node to be monitored
    const target = document.querySelector('body');
    // Configuration of the observer:
    const config = {
        attributes: true,
        childList: true,
        characterData: true
    };

    // Pass in the target node, as well as the observer options
    this.Observer.observe(target, config);
}

protected OnDestroy() {
    this.Observer.disconnect();
}
```
