# 将Hugo生成的静态网页部署到云服务器


# 将Hugo生成的静态网页部署到云服务器

以CentOS 7.6云服务器为例

1. 使用Xshell登录云服务器或使用WebShell登录云服务器(在服务器提供商的控制台登录)

2. 安装httpd服务

```bash
yum -y install httpd
```

安装完毕后，出现如下目录结构

```
/etc/httpd/conf/httpd.conf # 主配置文件
/var/www/html # 默认网站根目录
```

3. 启动httpd服务

```bash
service httpd start
```

4. 使用Xftp连接云服务器，将Hugo生成的public目录下的静态网页传输至`/var/www/html/`目录下
   ![image-20200719185353829](images/image-20200719185353829.png)

5. 通过http://云服务器IP地址/即可访问
