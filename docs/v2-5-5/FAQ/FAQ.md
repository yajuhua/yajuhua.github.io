
## 兼容性
> v1.x.x 与 v2.x.x 不兼容


## 忘记密码

### 进入数据卷目录

```shell
[root@centos7 ~]# docker volume inspect podcast2
[
    {
        "CreatedAt": "2024-03-23T19:57:47+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/podcast2/_data",
        "Name": "podcast2",
        "Options": null,
        "Scope": "local"
    }
]
[root@centos7 ~]# cd /var/lib/docker/volumes/podcast2/_data
[root@centos7 _data]# ls
cert  config  database  logs  plugin  resources  tmp
[root@centos7 _data]# cd config/
```

### 修改config.json

```shell
#改成true
{"initUserNameAndPassword":true}
```

### 重启后将恢复默认用户名和密码

> 用户名: admin  
> 密码: 123456

## 开启HTTPS

> 目前仅支持通过上传证书和密钥文件来实现

### 文件格式要求

```shell
# 证书文件格式必须是crt
# 密钥文件格式必须是key
```

### 重启后并以https访问

<br>

## 更新podcast2

> 数据保留

```shell
# 停止容器
docker stop podcast2

# 删除容器
docker rm podcast2

# 删除本地镜像
docker rmi yajuhua/podcast2:latest

# 拉取最新镜像
docker pull yajuhua/podcast2:latest

# 创建新的容器
docker run -id --name=podcast2 \
-p 8088:8088 \
--restart=always \
--mount source=podcast2,destination=/data \
yajuhua/podcast2:latest
```

## 重新开始

> 如果使用最新版都无法解决，可以试试删除数据

```shell
# 停止容器
docker stop podcast2

# 删除容器
docker rm podcast2

# 删除本地镜像
docker rmi yajuhua/podcast2:latest

# 删除数据
docker volume rm podcast2

# 拉取最新镜像
docker pull yajuhua/podcast2:latest

# 创建新的数据卷
docker volume create podcast2

# 创建新的容器
docker run -id --name=podcast2 \
-p 8088:8088 \
--restart=always \
--mount source=podcast2,destination=/data \
yajuhua/podcast2:latest
```

## 项目在线更新
podcast2 v2.4.0开始支持在线更新，国内需要设置代理，也可以直接Github加速站。
![设置GitHub加速站](../images/githubProxy.jpg)
下载完成后重启即为新版本

## 插件列表无法加载
插件列表的json数据是由cloudflare pages提供，可能存在无法访问的情况，下面有两个解决方法
### 1.更换插件仓库Url
[https://plugin.lancarjaya.eu.org/metadata.json](https://plugin.lancarjaya.eu.org/metadata.json)
![自定义插件仓库Url](../images/customzePluginRepo.jpg)
### 2.上传本地插件
[下载插件](https://github.com/yajuhua/generate-plugin-metadata-action/tree/master/v2)<br>
上传插件
![上传插件](../images/uploadPlugin.jpg)
## 插件bug或失效

由于插件并非使用官方接口，存在不稳定性。若发现插件失效，请提交[issues](https://github.com/yajuhua/podcast2/issues/new?assignees=&labels=bug&projects=&template=bug-report.yaml)

---