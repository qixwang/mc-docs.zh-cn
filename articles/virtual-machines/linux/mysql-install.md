---
title: 如何在 Azure 上安装 MySQL | Azure
description: 了解如何在 Azure 中的 Linux 虚拟机（Ubuntu 或 CentOS OS）上安装 MySQL 堆栈
services: virtual-machines-linux
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: 153bae7c-897b-46b3-bd86-192a6efb94fa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
origin.date: 02/01/2016
ms.date: 07/01/2019
ms.author: v-yeche
ms.openlocfilehash: 78d884eb30322da25fa815a8a09c8d3597e99d2d
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "67569762"
---
<!-- Remove the Red Hat family in meta properties-->
# <a name="how-to-install-mysql-on-azure"></a>如何在 Azure 上安装 MySQL
在本文中，将了解如何在运行 Linux 的 Azure 虚拟机上安装和配置 MySQL。

> [!NOTE]
> 必须已经有一个运行 Linux 的 Azure 虚拟机，才能完成本教程。 在继续操作前，请参阅 [Azure Linux VM 教程](quick-create-cli.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)创建并设置一个 Linux VM，其中 `mysqlnode` 为 VM 名称，`azureuser` 为用户。
> 
> 

在此情况下，请使用 3306 端口作为 MySQL 端口。  

我们将使用存储库包来安装 MySQL5.6，作为本文中的示例。 实际上，MySQL5.6 在性能上相对于 MySQL5.5 而言有更大的改进。 [此处](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/)提供更多信息。

## <a name="install-mysql56-on-ubuntu"></a>在 Ubuntu 上安装 MySQL5.6
我们将使用运行 Ubuntu 的 Linux VM。

### <a name="install-mysql"></a>安装 MySQL

通过切换到 `root` 用户来安装 MySQL Server 5.6：

```bash  
sudo su -
```

安装 mysql-server 5.6：

```bash  
apt-get update
apt-get -y install mysql-server-5.6
```

在安装过程中，会看到如下所示的对话窗口显示，要求设置 MySQL 根密码。需要在此处设置该密码。

![image](./media/mysql-install/virtual-machines-linux-install-mysql-p1.png)

再次输入密码进行确认。

![image](./media/mysql-install/virtual-machines-linux-install-mysql-p2.png)

### <a name="sign-in"></a>登录

在 MySQL Server 安装完成后，会自动启动 MySQL 服务。 可以使用 `root` 用户登录到 MySQL 服务器，并输入密码。

```bash  
mysql -uroot -p
```

### <a name="manage-the-mysql-service"></a>管理 MySQL 服务

获取 MySQL 服务的状态

```bash   
service mysql status
```

启动 MySQL 服务

```bash  
service mysql start
```

停止 MySQL 服务

```bash  
service mysql stop
```

重启 MySQL 服务

```bash  
service mysql restart
```

<!-- Not Avaiable on Red Hat OS 系列（例如 CentOS、Oracle Linux） -->

## <a name="install-mysql-on-centos"></a>在 CentOS 上安装 MySQL
此处会将 CentOS 与 Linux VM 一起使用。

### <a name="add-the-mysql-yum-repository"></a>添加 MySQL yum 存储库

切换到 `root` 用户：

```bash  
sudo su -
```

下载并安装 MySQL 发行包：

```bash  
wget https://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
yum localinstall -y mysql-community-release-el6-5.noarch.rpm
```

### <a name="enable-the-mysql-repository"></a>启用 MySQL 存储库
编辑以下文件，以便允许 MySQL 存储库下载 MySQL5.6 程序包。

```bash  
vim /etc/yum.repos.d/mysql-community.repo
```

将此文件的每个值更新为下面的值：

```  
\# *Enable to use MySQL 5.6*

[mysql56-community]
name=MySQL 5.6 Community Server

baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/

enabled=1

gpgcheck=1

gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
```

### <a name="install-mysql"></a>安装 MySQL 

从存储库安装 MySQL。

```bash  
yum install mysql-community-server
```

将安装 MySQL RPM 包和所有相关包。

## <a name="manage-the-mysql-service"></a>管理 MySQL 服务

检查 MySQL 服务器的服务状态：

```bash  
service mysqld status
```

检查 MySQL 服务器的默认端口是否正在运行：

```bash  
netstat  -tunlp|grep 3306
```

启动 MySQL 服务器：

```bash
service mysqld start
```

停止 MySQL 服务器：

```bash
service mysqld stop
```

将 MySQL 设置为在系统启动时启动：

```bash
chkconfig mysqld on
```

## <a name="install-mysql-on-suse-linux"></a>在 SUSE Linux 上安装 MySQL

我们在这里会通过 OpenSUSE 来使用 Linux VM。

### <a name="download-and-install-mysql-server"></a>下载并安装 MySQL Server

通过以下命令切换到 `root` 用户：  

```bash  
sudo su -
```

下载并安装 MySQL 程序包：

```bash  
zypper update
zypper install mysql-server mysql-devel mysql
```

### <a name="manage-the-mysql-service"></a>管理 MySQL 服务

检查 MySQL 服务器的状态：

```bash  
rcmysql status
```

检查 MySQL 服务器的默认端口是否正在运行：

```bash  
netstat  -tunlp|grep 3306
```

启动 MySQL 服务器：

```bash
rcmysql start
```

停止 MySQL 服务器：

```bash
rcmysql stop
```

将 MySQL 设置为在系统启动时启动：

```bash
insserv mysql
```

## <a name="next-step"></a>后续步骤
有关详细信息，请参阅 [MySQL](https://www.mysql.com/) 网站。

<!-- Update_Description: update meta properties, wording update-->