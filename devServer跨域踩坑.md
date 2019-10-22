### devServer跨域踩坑  
 
在vue或者webpack中代理的配置是相同的  

>  
```  
devServer: { 
  proxy: {  
    '/api': {  
      target: 'http://172.16.1.93:8080',
      changeOrigin: true,
      secure: false
    }  
  }  
}  
// target 表示要请求数据的地址（即代理转发后的真正地址）
```

但要注意的是  

1. 使用axios进行请求的时候，不能设置请求地址，否则报跨域错误

> 错误：
```
const baseURL = '127.0.0.1/api'
axios.defaults.baseURL = baseURL;
```
> 正确
```
const baseURL = '/api'
axios.defaults.baseURL = baseURL;
```
