# js removeEventListener失效的问题

#### 事情经过

   我想实现一个弹出框，点击空白处隐藏弹出框效果。

   隐藏思路： document监听click事件，将出发事件的元素进行隐藏。

```
    /* toggleRename([HtmlObject], 1) */
    toggleRename(event, bool) {
      console.log('toggleRename')
      if (bool) {
        this.listenClose(event)
        event.target.classList.add('active')
      } else {
        event.target.classList.remove('active')
        document.removeEventListener('click', this.toggleRename)
      }
    }
    listenClose(event) {
      // 给事件处理函数传参数， 函数执行时事件event对象会做为最后一个参数，相当于toggleRename(event, 0, $event)
      document.addEventListener('click', this.toggleRename.bind(null, event, 0))
    }
```
结果，这里removeEventListener失效了，网上查找后， 有说法是事件处理函数的执行上下文被改变后会无法清除


#### 解决
```
    toggleRename(event, bool) {
      console.log('toggleRename', bool)
      if (bool) {
        this.listenClose(event)
        event.target.classList.add('active')
      } else {
      }
    },
    listenClose(event) {
      function handler () {
        event.target.classList.remove('active')
        document.removeEventListener('click', handler)
      }
      document.addEventListener('click', handler)
      return handler
    }
```
