<h2 id="LSlRI">准备</h2>
随意注册一个cpolar账户，信息乱填

下载对应的版本，解压，然后连接自己专属的token，启动

```shell
# 连接账户
./cpolar authtoken xxx
# 运行
./cpolar [http/tcp] [端口]
# 如，22端口
./cpolar tcp 22
# 80端口
./cpolar tcp 80
```

idx使用google登录即可

选择实例，开启实例

> 注意：idx限制很多，机器休眠后，很多配置失效，也就是恢复原始状态，并且很多服务都进行了限制和屏蔽，！！！设置的策略也会重置，比如配置了sshd_config可以进行ssh连接；文件不会清掉（唯一的一个好处）
>

> 注意：每次用的时候记得加强密码，因为要是与cpolar联动时ip和端口都在暴露面
>

<h2 id="R3Tip">配置IDX</h2>
切换到root

```shell
sudo -i
```

！！！然后每次连接都要添加强密码，root和user都要！！！

![](https://cdn.nlark.com/yuque/0/2025/png/42377158/1752145466215-ef064d55-6d46-43a3-95b4-3eb97fd04198.png)

![](https://cdn.nlark.com/yuque/0/2025/png/42377158/1752145553722-b25c64a1-24c5-4453-ba45-9f6f09d4d966.png)

修改/etc/ssh/sshd_config文件，增加可root登录和需要密码登录

```shell
# 允许root登录
PermitRootLogin yes
# 允许密码登录
PasswordAuthentication yes
# 不允许空密码
PermitEmptyPasswords no
PrintLastLog no
PrintMotd no
PubkeyAuthentication no
Subsystem sftp /usr/lib/openssh/sftp-server
UsePAM yes
```

![](https://cdn.nlark.com/yuque/0/2025/png/42377158/1752145382556-32e3e6b7-03ca-4f01-8b05-d495ee49dafc.png)

此时我们先查看ssh的服务状态

```shell
systemctl status ssh
```

![](https://cdn.nlark.com/yuque/0/2025/png/42377158/1752144432636-f774f47c-57df-4b21-bd13-ae4b94f321a5.png)

masked这是屏蔽的状态，解除屏蔽

```shell
systemctl unmask ssh.service
systemctl unmask ssh.socket
```

![](https://cdn.nlark.com/yuque/0/2025/png/42377158/1752144689875-54cc81cc-d2a4-4729-bcb5-82e09f508f15.png)

此时重启服务，可能还是会显示错误信息

```shell
systemctl restart ssh
```

![](https://cdn.nlark.com/yuque/0/2025/png/42377158/1752144769904-09545e9d-4697-402d-be8f-aa5c2038cf2e.png)

经过多次尝试，发现22端口从开始就被占用

```shell
sudo ss -ltnp | grep :22
```

![](https://cdn.nlark.com/yuque/0/2025/png/42377158/1752144859660-70a259c4-8c93-40d8-98d5-cd97ab2c47fa.png)

pid为529，kill掉该进程，然后重启服务

```shell
kill 529
systemctl restart ssh
systemctl status ssh
```

![](https://cdn.nlark.com/yuque/0/2025/png/42377158/1752144970218-e3d1c772-4b97-4aed-b140-74ca94dde49b.png)

![](https://cdn.nlark.com/yuque/0/2025/png/42377158/1752145060936-a908f56a-9d4a-4fd0-a405-73909fe0e8e4.png)

此时ssh服务成功开启

现在本地测试是否能以root用户连接ssh

![](https://cdn.nlark.com/yuque/0/2025/png/42377158/1752145523076-e78a771c-2edf-429b-b87b-6714394b8d34.png)

<h2 id="zEHYL">启动cpolar</h2>
去到该目录下

![](https://cdn.nlark.com/yuque/0/2025/png/42377158/1752145628842-e1910d7d-86c2-41b3-93ae-59174a123e8e.png)

token：[https://dashboard.cpolar.com/get-started](https://dashboard.cpolar.com/get-started)

```shell
# 连接账户
./cpolar authtoken xxx
# 运行
./cpolar [http/tcp] [端口]
# 如，22端口
./cpolar tcp 22
```

![](https://cdn.nlark.com/yuque/0/2025/png/42377158/1752145714797-038635fd-580b-468f-9571-5165f07d12f5.png)

![](https://cdn.nlark.com/yuque/0/2025/png/42377158/1752145806168-ea1d6259-3b49-42e4-b763-2618d367a2da.png)

查看状态[https://dashboard.cpolar.com/status](https://dashboard.cpolar.com/status)

![](https://cdn.nlark.com/yuque/0/2025/png/42377158/1752145857999-ddd32534-124b-4dda-86ac-2b4bde9a6236.png)

用这个域名/ip+端口，即可进行ssh连接

<h2 id="ymlCp">使用</h2>
```shell
ssh root@[域名/ip] -p [端口]
```

![](https://cdn.nlark.com/yuque/0/2025/png/42377158/1752146032179-1d64ab90-af35-4357-9971-61876d6b863b.png)

或者就是用连接工具配置ssh来进行连接

~~~

