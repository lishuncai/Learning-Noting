# 浏览器click事件同步、异步、微任务、宏任务问题

```
        document.body.addEventListener('click',()=>{
            console.log('listener1');
            Promise.resolve().then(()=>console.log('micro task1'))
        })
        document.body.onclick = ()=>{
            console.log('listener2');
            Promise.resolve().then(()=>console.log('micro task2'))
        }
        document.body.click();
        
        // 验证同步、异步
        for (var i = 0; i<10000; i++) {
            console.log(i)
            new Promise((res, rej)=>{
                res('promise')
            }).then((data)=>{
                console.log(data)
            })
        }

```

##### 结论：分js触发和用户点击两种情况，自动触发是微任务，鼠标点击是宏任务
