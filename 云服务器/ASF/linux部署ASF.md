# linux部署ASF

## 1.安装.Net

> **注意：Ubuntu 22.04下直接安装dotnet可能出问题，是因为Ubuntu包和微软官方的包会冲突，要解决冲突，建议使用微软的官方包，如果不是Ubuntu系统则可以忽略下方的1）和2）**

1）删除本地的包

```
sudo apt remove dotnet*
sudo apt remove aspnetcore*
sudo apt remove netstandard*
```

2）在`/etc/apt/preferences.d`中创建一个pref文件，如`99microsoft-dotnet.pref`，在里面写入如下内容：

```
Package: *
Pin: origin "packages.microsoft.com"
Pin-Priority: 1001
```

3）安装依赖

- `ca-certificates`（标准可信 SSL 证书，用于 HTTPS 连接）
- `libc6`（`libc`）
- `libgcc-s1`（`libgcc1`、`libgcc`）
- `libicu`（`icu-libs`，您的发行版上的最新版，例如 `libicu72`）
- `libgssapi-krb5-2`（`libkrb5-3`、`krb5-libs`）
- `libssl1.1`（`libssl`、`openssl-libs`，您的发行版上的最新版本，并最低为 `1.1.X`）
- `libstdc++6`（`libstdc++`，`5.0` 或更高版本）
- `zlib1g`（`zlib`）

4）安装.Net 8.0(项目未来可能会采用更高版本)

```
sudo apt update
sudo apt install dotnet-sdk-8.0
```

5）测试.Net是否安装成功

```
dotnet --info
```



## 2.下载安装ASF

1）关于包选择，去[ASF发布页面](https://github.com/JustArchiNET/ArchiSteamFarm/releases)找到最新版本（即latest版本，不建议选择pre版本），选择ASF-generic.zip进行安装（若上面安装.Net环境不想搞，也可以选择直接安装linux系统包）

> Generic 包是一个与平台无关的版本，所以它不包含特定于计算机的代码。 所以使用这个包需要您的操作系统中已经安装有**合适版本**的 .NET 运行时环境。 我们都知道保证依赖始终是最新的这件事是十分令人烦恼的，所以这个包主要面向那些**已经在使用** .NET，不想为了 ASF 对已有环境做单独复制的人。 Generic 包还允许您在**任何地方运行 ASF，只要您能够获得正常工作的 .NET 运行时环境**，不需要担心是否存在相应的 OS-specific 包。
>
> 对于一般用户甚至是高级用户，如果只是想要运行 ASF 而不想要深入了解 . NET 的技术细节，不推荐使用 Generic 包。 也就是说，如果您明白以上所讲的内容，那您就可以使用它，不然下面所介绍的 OS-specific 包才是最合适的。

```
wget https://github.com/JustArchiNET/ArchiSteamFarm/releases/download/*.*.*.*/ASF-generic.zip
```

指令中的`*.*.*.*`需要替换为当前最新版本号

2）进入一个目录，比如root，解压asf

```
cd root
mkdir asf
mv ASF-generic.zip asf
cd asf
unzip ASF-generic.zip
rm -f ASF-generic.zip
```

3）查看解压后的文件

```
ll
```

提升ASF权限

```
chmod +x ArchiSteamFarm.sh
```



## 3.配置ASFbot

1）在[ASF配置文件生成器](https://justarchinet.github.io/ASF-WebConfigGenerator/#/)中生成配置文件并下载，推送到linux中`/asf/config`目录下



## 4.启动ASF

1）返回asf目录

2）启动asf

```
screen -S asf
./ArchiSteamFarm.sh
```

启动后稍等一会，会弹出手机令牌验证提示，根据提示信息继续即可

3）screen常用操作

新建screen：

```
screen -S <name>
```

退出当前screen：`Ctrl+A+D`

查看所有screen：

```
screen -ls
```

重新进入screen：

```
screen -r <id/name>
```

关闭screen：

```
screen -X -S <id/name> quit
```



## 5.登录IPC

ToDo...