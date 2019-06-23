**1）首先检查系统中是否已经安装了MySQL**

```shell
sudo netstat -tap | grep mysql
```



**2）如果没有安装，则安装MySQL**

```shell
sudo apt-get install mysql-server mysql-client 
```

输入mysql数据库密码



**3）测试安装是否成功：**

```shell
sudo netstat -tap | grep mysql 
```

![img](D:/Youdao/YoudaoDownLoad/qq8DA30C90167564599C3E6292BAA35072/d2389a43fe6841b7924e669d1011e471/clipboard.png)

出现此画面,则安装成功



**4）也可通过登录MySQL测试**

```mysql
 mysql -uroot -p密码
```



**5）mysql服务启动、停止、重启**

启动数据库

```shell
service mysqld start 
```

停止数据库

```shell 
service mysqld stop 
```

重启数据库

```shell
service mysqld restart 
```



**6 )查看mysql监听地址**

`netstat -ano | grep 3306 `

![img](D:/Youdao/YoudaoDownLoad/qq8DA30C90167564599C3E6292BAA35072/c875ffbb85ea4134bc17a0d27bf606f9/clipboard.png)

如图,第二条为默认情况下监听的地址,所监听的是服务器内网地址

第一条是后来所配置,目的是监听所有地址,让外网的工具可以对数据库进行访问.



**7）其他操作**

加载 .sql文件

`source .sql文件 `

注意，需要创建数据库