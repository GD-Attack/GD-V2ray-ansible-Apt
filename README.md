# GD-V2ray-ansible-Apt
## License

This project is licensed under the MIT License.
See the [LICENSE](./LICENSE) file for details.

使用v2ray配合ansible来实现批量搭建科学上网代理服务器，来实现科学上网，自动化批量部署代理服务器节点，使用文档说明十分清楚，使用前需要购买境外云服务器，并且在主控节点配置ansible，文档中的代理节点只是作为演示使用，目前文档中的代理服务器已经关闭，请大家自行购买代理节点或寻找可用的可操作的代理节点进行搭建

如果需要使用导航器执行剧本，那么就是从百度网盘中下载这个ansible-rhel9.iso
通过网盘分享的文件：ansible-rhel9.iso
链接: https://pan.baidu.com/s/1aOcfeuU6pegFaxNyRbmHNA?pwd=8888 提取码: 8888 
--来自百度网盘超级会员v1的分享

v2ray.tar.gz这个压缩文件中包含roles角色和执行角色的playbook剧本，下载之后解压，将这个playbook剧本与roles放到/etc/ansible，通过playbook剧本运行这个角色就好，最好ansible.cfg与我文档中的配置相同，不然可能会出现报错信息






# GD-V2ray-ansible-Apt
## 开发者：xian xichun


GD-V2ray-ansible-Apt 是一个基于 Ansible 自动化运维剧本（Roles）的项目，旨在帮助管理员在 Linux 服务器上一键、批量部署高性能的 Xray 代理节点（支持 VLESS-REALITY、VMess 等主流安全协议），并能自动生成对应的 Clash / Mihomo 订阅配置文件。

---

🚀 项目概述：GD-V2ray-ansible-Apt

GD-V2ray-ansible-Apt 是专为 Linux 管理员和高阶用户设计的 Ansible 自动化运维项目。它能够极大地降低代理节点的部署门槛，实现“代码即基础设施”的现代化运维体验。

🛠️ 它能帮你做什么？

一键式批量部署：摆脱繁琐的手动敲命令和重复配置，通过 Ansible 剧本同时向多台服务器推送节点。

安全的协议支持：内置对 Xray 的深度支持，重点优化了高安全性的 VLESS-REALITY 协议，免备案、防探测。

无缝订阅管理：部署完毕后自动生成节点订阅内容，告别手动拼接节点链接的痛苦，让 Clash 客户端能够即刻连接。

🐧 适用环境

默认支持：kali / Ubuntu Linux / Debian 系列

可完美运行在 Debian / Ubuntu 系列上。

## 🌟 项目特点

* **自动化批量部署**：利用 Ansible Role 机制，支持对单台或多台目标服务器同时进行服务部署与初始化。
* **依赖自动安装**：自动安装并配置 HTTPD、Firewalld、Curl、OpenSSL 等基础环境组件。
* **动态 IP 自动校准**：内置开机自启服务，即使服务器重启更换公网 IP，也会自动更新 Clash 订阅文件中的节点 IP。
* **Clash 订阅托管**：自动生成 `clash.yaml` 并托管于 Web 服务，方便客户端直接导入或更新订阅。

---

## 📂 文件与结构说明

在下载或解压 `v2ray.tar.gz` 后，建议将相关文件放置于 `/etc/ansible` 目录下使用：

* **`roles/v2ray/`**：核心 Ansible 角色目录，包含安装 Xray、配置生成、启动服务及开机自启脚本等任务。
* **`v2ray.yaml`** *(或您的剧本文件名)*：调用 `v2ray` 角色的 Ansible Playbook 主剧本。
* **`ansible.cfg`**：Ansible 配置文件，建议使用项目提供的配置以防止运行出现逻辑或路径报错。

---

## 🚀 快速使用指南

### 1. 解压并将文件移动至 Ansible 目录

下载 `v2ray.tar.gz` 后，解压并将角色（`roles`）与剧本（`playbook`）移动到 `/etc/ansible/` 目录下：

```bash
# 解压压缩包
tar -zxvf v2ray.tar.gz

# 进入解压后的目录并将文件复制/移动到 /etc/ansible
cp -a roles/ /etc/ansible/
cp v2ray.yaml /etc/ansible/   # 请根据实际的剧本文件名修改
cp ansible.cfg /etc/ansible/  # 直接复制我文档中的ansible.cfg配置文件也可以

```

> **⚠️️ 注意事项**：请确保 `/etc/ansible/ansible.cfg` 的配置与项目推荐一致（例如关闭 `host_key_checking = False` 或指定正确的 `roles_path`），避免因权限或主机密钥验证导致报错。

---

### 2. 配置主机清单（hosts）

编辑 `/etc/ansible/hosts` 文件，添加需要部署的目标服务器 IP 地址及 SSH 认证信息(或者是直接再主机清单中做解析，也可以，直接在主机清单中写上被控节点的IP地址)：

```ini
[v2ray_servers]
192.168.1.100 ansible_ssh_user=root ansible_ssh_pass=YourPassword

```
---

### 3. 执行 Playbook 部署

进入 `/etc/ansible` 目录并运行 Playbook 剧本：

```bash
cd /etc/ansible

# 执行 Ansible 剧本
ansible-playbook v2ray.yaml 
ansible-navigator run v2ray.yaml -m stdout
```

---

## 📡 订阅与使用

1. **获取订阅**：部署完成后，终端输出日志中会显示生成的 Clash 订阅地址：
```text
http://<您的服务器公网IP>/clash.yaml

```


2. **服务器重启支持**：若服务器重启导致公网 IP 发生变更，无需重新运行 Ansible。只需将客户端订阅链接中的 IP 修改为新的公网 IP 并重新拉取订阅即可。
