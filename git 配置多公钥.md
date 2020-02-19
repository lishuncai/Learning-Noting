# window 系统 git 配置多公钥.md

[相关文章参考](https://segmentfault.com/a/1190000019271156)

#### 创建ssh文件夹
```
ssh-keygen
```

#### 进入ssh 目录
```
  cd ~/.ssh
```
#### 生成SSH-Key
```
 ssh-keygen -t rsa -C "email@gitee.com" -f ~/.ssh/id_rsa_gitee
 
 ssh-keygen -t rsa -C "email@github.com" -f ssh/id_rsa_github
```
#### 添加公钥
将生成的公钥（.pub后缀）分别拷贝到远程仓库管理平台

#### 添加配置问件
```
// 创建
touch config
// 编辑
vim config

# gitee 码云
Host gitee.com 
HostName gitee.com  
PreferredAuthentications publickey 
IdentityFile ~/.ssh/id_rsa_gitee 
#
# github
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_github

## Host 这个指明的是HOST地址,也就是项目的HostName
## HostName 就是访问的地址，如：https://gitee.com/   就是其HostName（IP地址，访问的码云的网页上的url地址）  （https://建议不要加上）
## 指明配置的是公钥
## 指定弓腰的位置及文件
```
