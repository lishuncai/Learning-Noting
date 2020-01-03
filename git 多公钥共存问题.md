### git 多公钥共存问题

[参考文章](https://www.zhihu.com/question/21402411)

错误类型： 
  Permission denied (publickey)
  
解决方法

`
   ssh-add -k [id_rsa]
` 

   (如果报错： Could not open a connection to your authentication agent.则优先执行 ssh-agent bash）
   
验证: ssh -T git@github.com
