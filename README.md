# Ansible-OpenStack 自动化部署项目

## 项目简介
本项目旨在通过Ansible自动化工具，简化在OpenStack云平台上虚拟机的创建及常用Web应用（Nginx、MySQL、PHP）的自动化安装与配置。适用于需要快速搭建LAMP/LEMP环境的开发、测试或教学场景。

## 目录结构
```
Ansible-OpenStack-main/
├── Create_vm_and_install_app/
│   ├── Create_vm.yaml         # 创建OpenStack虚拟机的Ansible剧本
│   ├── Install_app.yaml       # 安装Nginx、MariaDB、PHP的Ansible剧本
│   ├── nginx.conf.j2          # Nginx配置模板
│   └── php.ini.j2             # PHP配置模板
├── LICENSE
└── README.md
```

## 主要功能
- 一键创建OpenStack虚拟机实例，并自动获取IP。
- 自动安装并配置Nginx、MariaDB（MySQL兼容）、PHP。
- 自动下发Nginx和PHP的配置文件模板。
- 自动开放80/443端口，服务启动后可直接访问。

## 使用方法
1. **准备环境**：
   - 已安装Ansible。
   - 已配置好OpenStack API访问凭据（如环境变量或clouds.yaml）。

2. **创建虚拟机**：
   - 编辑`Create_vm_and_install_app/Create_vm.yaml`中的变量（如实例名、镜像、网络等），或通过`-e`参数传递。
   - 执行命令：
     ```bash
     ansible-playbook Create_vm_and_install_app/Create_vm.yaml
     ```
   - 剧本会输出新建虚拟机的公网IP。

3. **安装应用环境**：
   - 修改`Install_app.yaml`中的主机为新建虚拟机的IP。
   - 执行命令：
     ```bash
     ansible-playbook -i '<新虚拟机IP>,' Create_vm_and_install_app/Install_app.yaml
     ```

4. **访问服务**：
   - 部署完成后，可通过浏览器访问虚拟机IP，验证Nginx和PHP服务是否正常。

## 文件说明
- `Create_vm.yaml`：调用OpenStack模块创建云主机，等待SSH可用并输出IP。
- `Install_app.yaml`：在目标主机上安装Nginx、MariaDB、PHP，并下发配置文件。
- `nginx.conf.j2`：Nginx主配置文件模板，可根据需要自定义。
- `php.ini.j2`：PHP主配置文件模板，基于官方生产环境配置，可根据实际需求调整。

## 许可证
本项目采用Apache License 2.0，详见LICENSE文件。
