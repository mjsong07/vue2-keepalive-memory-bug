# vue2-keepalive-memory-leak-bug
The demo can reproduce the vue keepalive memory problem.
# operating Steps
1. npm install 
2. npm run serve 
3. Open the browser and input the http://localhost:8080/
4. Click the add button to create a1-component.
5. Click the add button to create a2-component.
6. Click the add button to create a3-component.
7. Click the add button to create a4-component ， now the usedJSHeapSize is about 330mb.
8. Click the  a4-component delete button to remove a4-component ，but the usedJSHeapSize still 330mb. 
9. See the usedJSHeapSize output is not to reduce, now the memory is leak.
 
# view the Operating steps through by operating_steps.gif
# view memory analysis by memory_analysis.gif
# explain
When you click the add button ,the usedJSHeapSize will increase about 80mb.
click four times ,the usedJSHeapSize total is about 330mb.
next click the a4-component delete button ，the usedJSHeapSize Should be reduce 80mb，but it didn't happen.
# memory analysis
use the chrome memory tool ，We can find The bug because
the a3-component.componentInstance.$vnode.parent.componentOptions.children[0] is a4-component.
so the a4-component can not be Memory free.

# fixed the bug 
I try to fixed the bug in the vue2.7.12 -> keepalive.js -> render() 
path: src\core\components\keep-alive.ts
code: line 127 insert fixed code.
```js
 //mounted，repair the keepalive memory bug code
    for (const key in this.cache) {
      const entry = this.cache[key]
      if (entry && vnode && entry.tag && entry.tag !== vnode.tag ) { 
        entry.componentInstance.$vnode.parent.componentOptions.children = []
        entry.componentInstance.$vnode.parent.elm = null
      }
    }
```
Use the code above,the keepalive memory leak is fixed.
If you can read chinese ，Open the link for https://juejin.cn/post/7153186266300252168