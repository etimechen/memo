# ubuntu bash shell使用指定用户启动tomcat

>当我们使用批量部署tomcat时，bash shell脚本是不可缺的。一般情况下tomcat以root方式启动，但考虑到安全性问题，有时候我们需要使用指定用户启动tomcat，实现方式有两种，一种是使用jsvc，一种是使用su - user "command"
## 安装tomcat
