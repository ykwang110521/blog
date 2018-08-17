### ubuntu 安装kong

#### 一、安装kong
```php
$ sudo apt-get update
$ sudo apt-get install openssl libpcre3 procps perl
$ sudo dpkg -i kong-community-edition-0.14.0.*.deb （wget https://bintray.com/kong/kong-community-edition-deb/download_file?file_path=dists/kong-community-edition-0.14.0.xenial.all.deb）
```

#### 二、添加kong用户及授权
运行系统用户"postgres"的psql命令，进入客户端：
```php
sudo -u postgres psql
```
创建用户"kong"并设置密码：
```php
postgres=# create user kong with password '123456';
```
创建数据库kong，所有者为kong：
```php
postgres=# create database kong owner kong;
```
将konhg数据库的所有权限赋予kong用户，否则kong只能登录psql，没有任何数据库操作权限：
```php
grant all privileges on database kong to kong; 添加用户及授权
```


#### 三、修改配置文件
复制cp /etc/kong/kong.conf.default /etc/kong/kong.conf

注释
```php
pg_host = 127.0.0.1             # The PostgreSQL host to connect to.
pg_port = 5432                  # The port to connect to.
pg_user = kong                  # The username to authenticate if required.
pg_password = kong                  # The password to authenticate if required.
pg_database = kong              # The database name to connect to.

admin_listen = 0.0.0.0:8001
```

#### 四、迁移
```php
kong migrations up -c /etc/kong/kong.conf
```

#### 五、启动kong
```php
sudo kong start -c /etc/kong/kong.conf
```

#### 六、运行
命令行执行
```php
curl -i http://localhost:8001
```
浏览器打开
```php
http://dev.kong.test:8001/
```
有显示字符串说安装成功，如：
![](https://github.com/ykwang110521/blog/blob/master/201808/assets/kong_3.png)

#### 七、安装kong-dashboard 
> （不支持0.14版本或更高，我就遇到这个坑，一开始安装了最新版本的kong 后发现不支持）
```php
sudo npm install -g kong-dashboard
```
![](https://github.com/ykwang110521/blog/blob/master/201808/assets/kong_1.png)
![](https://github.com/ykwang110521/blog/blob/master/201808/assets/kong_2.png)
浏览器打开http://dev.kong.test:8080/
![](https://github.com/ykwang110521/blog/blob/master/201808/assets/kong_4.png)

#### 八、命令
停止kong
```php
$ kong stop
```
重新加载kong
```php
$ kong reload
```
重启kong
```php
$ kong restart
```



