---
ms.openlocfilehash: 3b9618b434576acc6b628524e40f4e43f347d367
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63827116"
---
### <a name="install-via-composer"></a>通过 Composer 安装

1. [安装 Git][install-git]。 请注意，在 Windows 上，还需要向 PATH 环境变量添加 Git 可执行文件。 

2. 在项目的根目录中创建一个名为 **composer.json** 的文件并向其添加以下代码：

    ```
    {
        "repositories": [
            {
                "type": "pear",
                "url": "http://pear.php.net"
            }
        ],
        "require": {
            "pear-pear.php.net/mail_mime" : "*",
            "pear-pear.php.net/http_request2" : "*",
            "pear-pear.php.net/mail_mimedecode" : "*",
            "microsoft/windowsazure": "*"
        }
    }
    ```

3. 将 **[composer.phar][composer-phar]** 下载到项目根目录中。

4. 打开命令提示符并在项目根目录中执行以下命令

    ```
    php composer.phar install
    ```

### <a name="install-manually"></a>手动安装

若要手动下载并安装 Azure 的 PHP 客户端库，请执行下列步骤：

> [!NOTE]
> Azure 的 PHP 客户端库依赖于 [HTTP_Request2](http://pear.php.net/package/HTTP_Request2)、[Mail_mime](http://pear.php.net/package/Mail_mime) 和 [Mail_mimeDecode](http://pear.php.net/package/Mail_mimeDecode) PEAR 程序包。 若要解析这些依赖关系，建议的方法是使用 [PEAR 程序包管理器](http://pear.php.net/manual/en/installation.php)安装这些包。

1. 从 [GitHub][php-sdk-github] 下载包含库的 .zip 存档。 或者，分叉存储库并将其克隆到本地计算机。 后一种方式需要一个 GitHub 帐户并要求在本地安装 Git。

2. 将所下载的存档的 `WindowsAzure` 目录复制到应用程序目录结构中。

有关安装 Azure 的 PHP 客户端库的详细信息（包括有关安装为 PEAR 程序包的信息），请参阅[下载 Azure SDK for PHP][download-SDK-PHP]。

[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[download-SDK-PHP]: ../articles/php-download-sdk.md
[composer-phar]: http://getcomposer.org/composer.phar