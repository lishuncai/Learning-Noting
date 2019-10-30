### 场景一： 设置默认图片 ###

```
state: {
  avatar: require('@/assets/images/img_avatar.svg')  
}
```


### 场景二： 设置如何响应的获取vuex的对象属性值

vuex属性：
```
userInfo: null
```
异步请求并赋值：
```
setTimeout(()=>{
 state.userInfo = {
  userName: '深海时光'
 }
}, 1000)
```
vue组件绑定数据：
```
<template>
  <span>{{userInfos.userName || ''}}</span>
</template>

<script>
  export default {
    data () {
      return {}
    },
    computed: {
      userInfos () {
        return this.$store.state.userInfos || {}
      }
    }
  }
</script>
```
即通过计算属性监听vuex属性的状态，从而动态更新模板数据
