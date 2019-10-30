### vue项目部署路径问题 ###

当项目部署的路径不在域名根路径时，需要更改vue.config.js的publicPath属性为部署目录，
publicPath默认路径为'/'。

当然路由配置也是：
(切勿写死太多数据)
`  
  base:  process.env.BASE_URL
`
