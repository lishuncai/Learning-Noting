### vue 使用$refs获取不到dom的问题

当ref绑定的节点还未被渲染出来时， 是获取不到dom的

当节点被渲染之前获取不到时， 可用$nextTick解决：

```
  this.$nextTick(() => {
    console.log( this.$refs.img)
  })
  
```
