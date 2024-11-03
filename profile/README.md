# HACS 极速版

[![version](https://img.shields.io/github/v/release/hacs-china/integration)](https://github.com/hacs-china/integration/releases/latest?include_prereleases)
[![releases](https://img.shields.io/github/downloads/hacs-china/integration/total)](https://github.com/hacs-china/integration/releases)
[![stars](https://img.shields.io/github/stars/hacs-china/integration)](https://github.com/hacs-china/integration/stargazers)

[HACS](https://hacs.xyz)是一款优秀的 [Home Assistant](https://www.home-assistant.io) 集成商店，然而国人想要使用它下载插件或前端卡片却困难重重，主要原因就是国内的网络环境。
本项目使用了[gitmirror.com](https://gitmirror.com)和[fastgit.org](https://fastgit.org)等提供的Github代理服务，可以让大家更快的下载商店里的插件。

<a name="install"></a>
## 安装/更新

> 本项目是HACS官方集成的修改版，安装本项目会覆盖官方的集成，但是无需重新配置集成(共用一套配置)，因此你可以放心安装。如果想切换到官方版本，使用官方的shell命令再安装即可。
>
> 以下几种方法任选其一！

#### 方法1️⃣: 使用命令安装

```shell
wget -O - https://get.hacs.vip | bash -
```

- 如果是haos/hassio/supervised版本的HA，可直接在宿主机或`Terminal & SSH`加载项中执行上面的命令
- 如果是core/docker版本的HA，需要ssh登陆宿主机后，并cd进入到HA配置目录再执行安装命令

#### 方法2️⃣: [`加载项安装器: https://hacs.vip/get-addon`](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgitee.com%2Fhacs-china%2Faddons)

> 需要HAOS或Supervised版本的HA

1. 添加加载项仓库 [`https://gitee.com/hacs-china/addons`](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgitee.com%2Fhacs-china%2Faddons)
2. 找到`HACS极速版安装器`并安装加载项
3. 启动加载项并观察日志
4. 重启HA

#### 方法3️⃣: [`upgrade`](https://my.home-assistant.io/redirect/developer_call_service/?service=hacs.upgrade)服务

> 需要已安装HACS极速版v1.33.0.3版本及以上

1. 在开发者工具中执行服务 [`service: hacs.upgrade`](https://my.home-assistant.io/redirect/developer_call_service/?service=hacs.upgrade)
2. 重启HA使更新后的HACS生效

#### 方法4️⃣: [`shell_command`](https://my.home-assistant.io/redirect/developer_call_service/?service=shell_command.update_hacs_china)服务

1. 复制代码到HA配置文件 `configuration.yaml`
    ```yaml
    shell_command:
      update_hacs_china: |-
        wget -O - https://get.hacs.vip | bash -
    ```
2. 重启HA使配置生效
3. 在开发者工具中执行动作 [`action: shell_command.update_hacs_china`](https://my.home-assistant.io/redirect/developer_call_service/?service=shell_command.update_hacs_china)
4. 再次重启HA使更新后的HACS生效

#### 方法5️⃣: [`Docker安装`](https://hub.docker.com/r/hasscc/hacs)

> 仅针对首次安装全新**Docker**版本的HA

1. 使用命令方式安装
    ```bash
    docker run -d \
      --name homeassistant \
      --privileged \
      --restart=unless-stopped \
      -e TZ=Asia/Shanghai \
      -v /PATH_TO_YOUR_CONFIG:/config \
      -v /run/dbus:/run/dbus:ro \
      --network=host \
      hasscc/hacs:stable
    ```
2. 使用Compose安装
    ```yaml
    services:
      homeassistant:
        container_name: homeassistant
        image: hasscc/hacs:stable
        volumes:
          - /PATH_TO_YOUR_CONFIG:/config
          - /etc/localtime:/etc/localtime:ro
          - /run/dbus:/run/dbus:ro
        restart: unless-stopped
        privileged: true
        network_mode: host
    ```
3. 启动后[添加HACS集成](https://my.home-assistant.io/redirect/config_flow_start/?domain=hacs)

#### 方法6️⃣: 手动安装

- [点击这里下载](https://github.com/hacs-china/integration/releases/latest/download/hacs.zip)安装包并解压 (如果下载不了请点[这里](https://ghproxy.com/github.com/hacs-china/integration/releases/latest/download/hacs.zip)或[这里](https://hub.fastgit.xyz/hacs-china/integration/releases/latest/download/hacs.zip))
- 通过samba/ftp进入HA配置目录，通常为以下目录：
  - `/usr/share/hassio/homeassistant` haos/hassio宿主机
  - `/config` haos/hassio的`Samba`或`Terminal & SSH`加载项
  - `$HOME/.homeassistant` 以core方式安装的HA默认配置目录
  - docker安装的HA为`-v`参数后面映射的目录
- 在HA配置目录下创建`custom_components`文件夹 (如果已有请忽略)
- 在`custom_components`目录下创建`hacs`文件夹 (如果已有请删除重新创建)
- 將解压出来的文件复制到刚创建的`hacs`文件夹
- 重启HA
- [添加HACS集成](https://my.home-assistant.io/redirect/config_flow_start/?domain=hacs) (仅首次安装)

> ⚠️ 请不要通过下图中的位置下载HACS，会缺少文件
> ![download hacs](https://user-images.githubusercontent.com/4549099/157629602-422a7bbe-7588-4a81-803e-b295491d78fe.png)


<a name="proxy"></a>
<a name="mirror"></a>
## 代理

> **Note**
> 
> 自v1.27.1.3开始，HACS极速版支持自定义Github API地址，如果你的HACS无法加载集成列表和集成详情，修改此选项会有所改善。
> 此前的版本仅能解决集成下载不了，而该版本后能解决大部分Github访问不了导致的大部分问题。
> 
> 不过遗憾的是，首次安装HACS时的授权过程仍然还不能被加速，如果你在授权过程中一直转圈，请稍后再试或使用其他科学的方式。

- 社区提供的免费代理：
  - `https://ghapi.hacs.vip` - [@al-one](https://github.com/al-one)
  - `https://ghapi-cf.hacs.vip/api` - [@al-one](https://github.com/al-one)
  - `https://hacs-china.chrome7.com/api` - [@goxofy](https://github.com/goxofy)
  - `https://hacs-china.casen.tk/api` - [@CasenChan](https://github.com/CasenChan)

> **Note**
> 
> 以上地址由贡献者免费提供，是由`Cloudflare Worker`搭建，每个代理每天有10万次请求次数限制，请随机使用上面的代理。
> 我们建议你使用自己的域名创建代理，当然也可以使用[`freenom.com`](https://freenom.com)的免费域名。

- 创建自己的代理：
  - 登陆或注册[`Cloudflare`](https://cloudflare.com)添加自己的域名，并修改域名的NS记录
  - [创建`Worker`服务](https://dash.cloudflare.com/?account=workers)，选择`HTTP 处理程序`
  - 复制[`index.js`](https://raw.githubusercontent.com/hacs-china/gh-proxy/master/index.js)中的代码，并张贴至Worker的代码编辑器中
  - 部署并在触发器中添加自定义域名，Worker分配的域名是无法被访问的
  - 访问`https://your.mirror.domain/api/`检查是否生效
  - 在HA的集成与服务页面找到已添加的HACS，点击`选项`
  - 填入地址`https://your.mirror.domain/api`


<a name="faq"></a>
## 常见问题

- [极速版和官方HACS的差别有那些？](https://github.com/hacs-china/integration/compare/main...china)


------

## 🎉 `Hassio`/`Supervisor`加载项(`Add-ons`)商店镜像：https://gitee.com/hassio 🎉
