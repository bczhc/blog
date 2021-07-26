## Docker的一些理解和基本命令实例

Docker是一个开源容器化平台，可以把部署环境打包在镜像中，从而更方便地移植。

**[Docker文档](https://docs.docker.com)**

### 镜像和容器

docker中的镜像是只读的，由一个镜像（images）可以创建出多个容器（containers），每个容器是可以做更改的，在里面搭建应用环境并测试运行。一般一个容器可作为一个软件单元，包括了一个软件及其依赖，这样就可以方便应用环境的移植。

docker中提供了很多的基础镜像，如ubuntu，centos等，这些镜像可以从Docker Hub拉取。

#### 镜像

##### 查看镜像

`docker images [OPTIONS] [REPOSITORY[:TAG]]`

```
[bczhc@bczhc-manjaro docker]$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
base         1.0       667bc9764ca8   42 hours ago   170MB
ubuntu       latest    c29284518f49   8 days ago     72.8MB
```

##### 在Docker Hub搜索镜像

`docker search [OPTIONS] TERM`或者上[Docker Hub](http://hub.docker.com)搜索。

```
[bczhc@bczhc-manjaro docker]$ docker search kali
NAME                                   DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
kalilinux/kali-rolling                 Official Kali Linux image (weekly snapshot o…   301
kalilinux/kali                         Image built from the last official release      76
linuxkonsult/kali-metasploit           Kali base image with metasploit                 71                   [OK]
booyaabes/kali-linux-full              Kali image with kali-linux-full metapackage …   31                   [OK]
...


[bczhc@bczhc-manjaro docker]$ docker pull kalilinux/kali-rolling
Using default tag: latest
latest: Pulling from kalilinux/kali-rolling
20de899349e6: Pull complete 
Digest: sha256:d43e531ec8c89f712f4add7a23b4ccf6c84d0fa552afa5fdd3488a954734e86f
Status: Downloaded newer image for kalilinux/kali-rolling:latest
docker.io/kalilinux/kali-rolling:latest
```

##### 删除镜像

`docker rmi [OPTIONS] IMAGE [IMAGE...]`

```
[bczhc@bczhc-manjaro docker]$ docker images
REPOSITORY               TAG       IMAGE ID       CREATED        SIZE
base                     1.0       667bc9764ca8   43 hours ago   170MB
ubuntu                   latest    c29284518f49   8 days ago     72.8MB
kalilinux/kali-rolling   latest    2a25ab3f682f   10 days ago    125MB
[bczhc@bczhc-manjaro docker]$ docker rmi base:1.0 
Untagged: base:1.0
Deleted: sha256:667bc9764ca84194ab2a7e1aca0f99901245e2733e48afc441e6ab21ab782452
Deleted: sha256:fc0f9d8985ccc19852bba10ed727e806aaf68b5d6796110cf313670087f26c9e
```

#### 容器

##### 创建容器

`docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`

```
[bczhc@bczhc-manjaro docker]$ docker images
REPOSITORY               TAG       IMAGE ID       CREATED        SIZE
base                     1.0       667bc9764ca8   43 hours ago   170MB
ubuntu                   latest    c29284518f49   8 days ago     72.8MB
kalilinux/kali-rolling   latest    2a25ab3f682f   10 days ago    125MB
[bczhc@bczhc-manjaro docker]$ docker run -it --name kali kalilinux/kali-rolling bash
┌──(root💀e32bcd9f43e0)-[/]
└─# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

┌──(root💀e32bcd9f43e0)-[/]
└─# exit
exit
```

##### 删除容器

`docker rm [OPTIONS] CONTAINER [CONTAINER...]`

```
[bczhc@bczhc-manjaro docker]$ docker ps -a
CONTAINER ID   IMAGE                    COMMAND   CREATED          STATUS                     PORTS     NAMES
e32bcd9f43e0   kalilinux/kali-rolling   "bash"    23 minutes ago   Exited (0) 7 minutes ago             kali
4d564e7daf6b   17de422fad2c             "bash"    43 hours ago     Exited (0) 13 hours ago              eager_jackson
9bf7e01b2c9b   ubuntu                   "bash"    43 hours ago     Exited (0) 43 hours ago              crazy_mccarthy
[bczhc@bczhc-manjaro docker]$ docker rm 4d56
4d56
[bczhc@bczhc-manjaro docker]$ docker ps -a
CONTAINER ID   IMAGE                    COMMAND   CREATED          STATUS                     PORTS     NAMES
e32bcd9f43e0   kalilinux/kali-rolling   "bash"    23 minutes ago   Exited (0) 7 minutes ago             kali
9bf7e01b2c9b   ubuntu                   "bash"    43 hours ago     Exited (0) 43 hours ago              crazy_mccarthy
```

##### 查看容器

使用`docker ps`查看当前正在运行的容器，用`docker ps -a`查看本地存在的容器，即所有容器，包括不在运行的。

```
[bczhc@bczhc-manjaro docker]$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[bczhc@bczhc-manjaro docker]$ docker ps -a
CONTAINER ID   IMAGE                    COMMAND   CREATED         STATUS                     PORTS     NAMES
e32bcd9f43e0   kalilinux/kali-rolling   "bash"    2 minutes ago   Exited (0) 2 minutes ago             kali
4d564e7daf6b   17de422fad2c             "bash"    43 hours ago    Exited (0) 12 hours ago              eager_jackson
9bf7e01b2c9b   ubuntu                   "bash"    43 hours ago    Exited (0) 43 hours ago              crazy_mccarthy
```

由于刚才的kali容器退出了，因此kali容器没有显示在`docker ps`中。

##### 运行容器

`docker start [OPTIONS] CONTAINER [CONTAINER...]`

```
[bczhc@bczhc-manjaro docker]$ docker start kali
kali
```

`<container>`占位参数就是一个容器的引用，可以为容器的名字（在创建容器时用`--name <name>`参数指定），也可以是唯一标识符（Docker使用的SHA256 ID）。

此时kali容器已经运行起来，使用`docker attach`连接终端。

```
[bczhc@bczhc-manjaro docker]$ docker ps
CONTAINER ID   IMAGE                    COMMAND   CREATED         STATUS         PORTS     NAMES
e32bcd9f43e0   kalilinux/kali-rolling   "bash"    8 minutes ago   Up 3 minutes             kali
[bczhc@bczhc-manjaro docker]$ docker attach kali
└─# 

┌──(root💀e32bcd9f43e0)-[/]
└─# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

只要容器不删除，在容器内的变更都会保存在本地。但是如果希望此时的变更可以长时间保存下来或者是作为一个单独的镜像，可以由这个镜像再在它的基础上开启容器。

##### 提交容器变更到新的镜像

`docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]`

首先基于把之前的kali容器打开并附着终端，并且创建一个文件

```
[bczhc@bczhc-manjaro docker]$ docker ps -a
CONTAINER ID   IMAGE                    COMMAND   CREATED          STATUS                     PORTS     NAMES
e32bcd9f43e0   kalilinux/kali-rolling   "bash"    32 minutes ago   Exited (0) 4 seconds ago             kali
9bf7e01b2c9b   ubuntu                   "bash"    43 hours ago     Exited (0) 43 hours ago              crazy_mccarthy
[bczhc@bczhc-manjaro docker]$ docker start kali
dokali
[bczhc@bczhc-manjaro docker]$ docker attach kali
└─# touch some
┌──(root💀e32bcd9f43e0)-[/]
└─# 
exit
```

然后commit

```
[bczhc@bczhc-manjaro docker]$ docker commit kali bczhc/kali:1.0
sha256:6fd4e33845556dad1b6858bd39515bb2e32b34d3ea9389d5e3842f36cea8bdb9
[bczhc@bczhc-manjaro docker]$ docker images
REPOSITORY               TAG       IMAGE ID       CREATED         SIZE
bczhc/kali               1.0       6fd4e3384555   2 seconds ago   125MB
ubuntu                   latest    c29284518f49   8 days ago      72.8MB
kalilinux/kali-rolling   latest    2a25ab3f682f   10 days ago     125MB
```

此时镜像中就有了一个新的镜像，基于这个新的镜像再启动一个新容器

```
[bczhc@bczhc-manjaro docker]$ docker run -it bczhc/kali:1.0 bash
┌──(root💀886e4ba7cd82)-[/]
└─# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  some  srv  sys  tmp  usr  var

┌──(root💀886e4ba7cd82)-[/]
└─# 
```

就可以发现保留了刚才的变更。并且用`docker inspect [OPTIONS] NAME|ID [NAME|ID...]`查看这个新镜像，可以发现它有两层，这里的层就像git中的commits一样。

```
[bczhc@bczhc-manjaro docker]$ docker inspect bczhc/kali:1.0
...

bczhc@bczhc-manjaro docker]$ docker inspect bczhc/kali:1.0 | jq .[0].RootFS
{
  "Type": "layers",
  "Layers": [
    "sha256:55f188416add482e5d6b51e09797ec82d35cb52c276c1c174804fefacc7506c9",
    "sha256:b841b9ce6ea9f32ea1243e1e434dd7884ea26a37548f9d66e25763770da91802"
  ]
}
```

再来inspect最初创建容器基于的镜像`kalilinux/kali-rolling `

```
[bczhc@bczhc-manjaro docker]$ docker inspect kalilinux/kali-rolling:latest | jq .[0].RootFS
{
  "Type": "layers",
  "Layers": [
    "sha256:55f188416add482e5d6b51e09797ec82d35cb52c276c1c174804fefacc7506c9"
  ]
}
```

它们的输出都为JSON，经过一些处理之后找到了相应的信息，发现`bczhc/kali`镜像比`kalilinux/kali-rolling`镜像多了一层，那一层就是我们刚才的改动层。

#### 其它

##### 审查对象

`docker inspect [OPTIONS] NAME|ID [NAME|ID...]`

审查可以输出一些有关docker对象的低等级信息，这些信息很多时候很有用处。