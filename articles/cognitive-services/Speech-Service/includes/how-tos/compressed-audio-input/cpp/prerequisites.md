---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
origin.date: 03/09/2020
ms.date: 03/16/2020
ms.author: v-tawe
ms.openlocfilehash: 8d6a072cf86bdf443c9f7646586b0b0aa938f475
ms.sourcegitcommit: b2f2bb08ab1b5ccb3c596d84b3b6ddca5bba3903
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "80151699"
---
处理压缩音频是使用 [GStreamer](https://gstreamer.freedesktop.org) 实现的。 出于许可原因，GStreamer 二进制文件未编译，也未与语音 SDK 链接。 开发人员需要安装几个依赖项和插件。

# <a name="ubuntu-1604-1804-or-debian-9"></a>[Ubuntu 16.04、18.04 或 Debian 9](#tab/debian)

```sh
sudo apt install libgstreamer1.0-0 \
gstreamer1.0-plugins-base \
gstreamer1.0-plugins-good \
gstreamer1.0-plugins-bad \
gstreamer1.0-plugins-ugly
```

# <a name="rehl--centos"></a>[REHL/CentOS](#tab/centos)

```sh
sudo yum install gstreamer1 \
gstreamer1-plugins-base \
gstreamer1-plugins-good \
gstreamer1-plugins-bad-free \
gstreamer1-plugins-ugly-free
```

> [!NOTE]
> 在 RHEL/CentOS 上，按照[如何配置 OpenSSL for Linux](../../../../how-to-configure-openssl-linux.md) 中的说明进行操作。

---