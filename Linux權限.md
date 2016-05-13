---
title: 用户及文件权限管理
tags: Linux,命令
grammar_cjkRuby: true
---

####添加用户
sudo adduser linus

####删除用户
sudo deluser linus --remove

####`su`，`su-` 与 `sudo`

`su <user>`可以切换到用户user，执行时需要输入目标用户的密码，sudo <cmd>可以以特权级别运行cmd命令，需要当前用户属于sudo组，且需要输入当前用户密码。`su - <user>`命令也是切换用户，同时环境变量也会跟着改变成目标用户的环境变量。

####`sudoers`用户组
默认情况下在sudo用户组里的可以使用sudo命令获得root权限。shiyanlou 用户也可以使用 sudo 命令，为什么这里没有显示在 sudo 用户组里呢？可以查看下 `/etc/sudoers.d/shiyanlou` 文件，我们在` /etc/sudoers.d`目录下创建了这个文件，从而给 shiyanlou 用户赋予了 sudo 权限：

####查看权限
`/etc/group`
/etc/group 的内容包括用户组（Group）、用户组口令、GID 及该用户组所包含的用户（User），每个用户组一条记录。格式如下：

>`group_name:password:GID:user_list`

```
shiyanlou:x:5000:
linus:x:1000:
```
你看到上面的 password 字段为一个 'x' 并不是说密码就是它，只是表示密码不可见而已。

#### `usermod` 命令添加用户组

这里我用 `shiyanlou` 用户执行 sudo 命令将 `linus`添加到 sudo 用户组，让它也可以使用 sudo 命令获得 root 权限
```
shiyanlou:~/ $ su shiyanlou
shiyanlou:~/ $ groups linus
linus : linus
shiyanlou:~/ $ sudo usermod -G sudo linus
shiyanlou:~/ $ groups linus
linus : linus sudo
```
![](https://dn-anything-about-doc.qbox.me/linux_base/3-9.png/logoblackfont)
![](https://dn-anything-about-doc.qbox.me/linux_base/3-10.png/logoblackfont)

####`chown`更改文件的拥有者或组
```
sudo chown shiyanlou iphone6 #更改iphone6所有者为shiyanlou用户
```
###修改文件权限 `chmod`
 1. 方式一:二进制数字表示
![](https://dn-anything-about-doc.qbox.me/linux_base/3-14.png/logoblackfont)
每个文件的三组权限（拥有者，所属用户组，其他用户,记住这个顺序是一定的）就对应这一个 "rwx"，也就是一个 '7' ,所以如果我要将文件“iphone6”的权限改为只有我自己可以用那么就这样:
```
$ chmod 700 iphone6
```
 2. 方式二加减赋值操作
```
$ chmod go-rw iphone
```

>`'g''o'`还有`'u'`，分别表示`group`，`others`，`user`，`'+'`，`'-'` 就分别表示`增加`和`去掉`相应的权限。






