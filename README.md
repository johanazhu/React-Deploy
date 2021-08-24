# React-Deploy

## 目的 

通过 Github Action 把一个单页面应用部署到 云服务器上

## 参考资料

具体可参考 [使用Github Action 部署项目到云服务器](https://zhuanlan.zhihu.com/p/107545396) 中的操作，我讲讲在使用这位大哥写的 `SFTP-Deploy-Action@v1.0 ` action 时遇到的坑

## 常见问题

首先遇到 `Load key "../private_key.pem": invalid format` 的错误。

解决方案 `ssh-keygen -m PEM` 生成 新key

我遇到类似的问题，以为要更新我的私钥，后来发现不是，是因为我的私钥是 openssh，改成 rsa 就没报这个错误了

#### 错误二：`Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).`

免登录问题

具体步骤较多，简单来说，就是 `.ssh` 文件下新建一个 authorized_keys 文件，将公钥放到其中

并去 `/etc/ssh/sshd_config` 中修改一些配置，实现免登录

这里不细讲，具体可以 Google 查一下

### 错误三：

```shell
Warning: Permanently added '_\*\*' (ECDSA) to the list of known hosts.
sftp> put -r ./build/_ /home/johan/www/react-app
Multiple paths match, but destination "/home/johan/www/react-app" is not a directory
```

文件路径不对

#### 错误四：

```shell
Couldn't canonicalize: No such file or directory
Unable to canonicalize path "/home/johan/www/react-app/static"
```

找不到 static 文件，新生成的 static 文件不能放入项目中，那就新建一个

去项目目录下 `mkdir static`

再编译，成功

![编译成功](https://i.loli.net/2021/08/24/ShPCcvQqxViWrpu.png)

