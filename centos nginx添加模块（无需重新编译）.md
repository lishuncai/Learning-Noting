# centos nginx添加模块(无需重新编译)

- 进入nginx源码目录

```
  $ cd nginx-1.3.2
```

- 查看以安装模块

```
  $ nginx -V
```

- 重新编译, 加入想要添加的模块

```
  $ ./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module --with-file-aio  --with-http_realip_module
```

- 安装（注意无需make install）

```
  make
```

- 备份旧的nginx程序
```
  $ cp /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginx.bak
```

- 把新的nginx程序覆盖旧的(重要), 如果报错,可能是nginx正在运行，需关闭
```
  nginx -s stop
```
```
  $ cp objs/nginx /usr/local/nginx/sbin/nginx
```

- 查看新模块添加成功
 ```
  $ nginx -V
 ```

- 重启
```
  nginx -s reload
```
