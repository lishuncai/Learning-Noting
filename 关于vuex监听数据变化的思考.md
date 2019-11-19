### 关于vuex监听数据变化的思考

先说明有这么一种场景：
  用户进入页面时，需要优先看见页面，即页面已经渲染，mounted，actived钩子已经执行。vue已经进行了第一次渲染。  
  此时store还没有全局基础数据，数据在http请求回来的路上。等数据回来后，再根据全局基础数据的作为参数进行组件展示数据的请求。 
  
  即：用户访问网站 ——> 请求全局基础数据 ——> 组件首次渲染 ——> 请求组件展示数据 ——> 完成组件渲染  
  
  此时存在的问题在于，请求组件展示数据时，有没有参数可以请求（参数来自全局基础数据）？  
   有： 有参数 ——> 请求组件数据 ——> 完成组件渲染  
   否： 等带参数回来 ——> 请求组件数据 ——> 完成组件渲染
  
  
  问：如何做到参数回来后马上请求组件数据请求
  
  方案1. 钩子函数触发时判断store是否有全局数据，如何没有组件内部请求全局数据 ，
  成功后触发回调函数进行第二次请求， 并把全局数据放入store。
  优点： 当该组件没有缓存时（详见vue：keep-alive）， 每次激活组后后执行请求，刷新视图，轻松的让视图的数据及时得到更新。
  缺点： 网络延迟时，组件频繁激活会发送多次请求。
  
  方案2. 组件激活前请求全局数据，组件激活后立刻判断是否有全局数据，没有则用watch监听数据。
  代码实现：
  ```
   watch: {
    '$store.state.globalData': {
      handler: function (value) {
        this.topAreaId = value
      },
      immediate: true // 先执行一次
    }
  ```
    优点： 无论组件是否被缓存，只要数据发生变换，即触发请求刷新视图，网络延迟时页面反应良好。
    缺点： 组件未被缓存时，组件频繁激活也会发送多次请求。组件被缓存时，再次打开将不会重新请求数据，视图得不到更新。
    需要配合activeted钩子，但和watch同一组件时，可能会重复请求。
    
  方案3. 和方案2同理，但实现方法不同，先看下相关代码：
  ```
  // comp.vue
  export default {
     async activated() {
      let infos = await this.$store.getters.userInfo();
      console.log(infos)
    },
  }
  
  // store.js
  new Vuex.Store({
    getters: {
      userInfo: (state) => (time) => {
      if (state.userInfo) return state.userInfo
        return new Promise(async (resolve) => {
          let data = await store.dispatch('getUserInfo')
          state.userInfo = data
          resolve(data)
        })
      }
    },
    actions: {
       getUserInfo(context) {
        return new Promise((resolve, reject) => {
          server.request('getCompInfo')
          .then(res => {
            resolve(res)
          })
          .catch(err => {
            reject(err)
          })
        })
      }
    }
  })
  ```
  方案3采用异步的方式，将判断交给store处理，组件只要等待数据即可。
  注意： 经过实际开发发现： vuex 的getter会缓存上一次结果，所以后续访问getters都会返回上一次结果，
  所以如果不用函数，将不会发生请求，也就得不到最新的数据。
  
  优点： 将复杂的数据判断交给store处理，多组件都可以经过这一流程，组件只需等待确认结果，然后立即执行下一步请求。
  可灵活处理触发请求代码放在哪个钩子里，组件本身无需担心会不会造成重复请求，是否需要重复或限制重复请求可在store里进行处理。
  
  缺点： 代码量大，需封装好, 实现复杂， 其他弊端待观察。

  方案3加强版： 处理重复请求；
  当请求没有结果，或者说请求还没有结果的时候，单纯通过判断是否存在我们需要的结果来进一步执行代码，这时容易出现重复请求的情况，
  参考promise对象的状态变换， 可以通过给接受结果的变量进行状态控制，如何状态处于 “请求中...”，则不用在请求一次了。
