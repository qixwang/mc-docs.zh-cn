---
title: 对 Azure 容器注册表中的存储库的权限
description: 创建一个令牌，使其权限范围限定于注册表中的特定存储库，用来拉取或推送映像，或执行其他操作
ms.topic: article
origin.date: 02/13/2020
ms.date: 05/20/2020
ms.author: v-yeche
ms.openlocfilehash: a0c26b3c81ff443ecca7147d7d43568c32846d99
ms.sourcegitcommit: 2d8950c6c255361eb6c66406988e25c69cf4e0f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2020
ms.locfileid: "83392650"
---
<!--Verified successfully-->
# <a name="create-a-token-with-repository-scoped-permissions"></a>创建一个具有存储库范围权限的令牌

本文介绍了如何创建令牌和范围映射，以便在容器注册表中管理存储库范围的权限。 通过创建令牌，注册表所有者可以为用户或服务提供范围限定于存储库且有时间限制的访问权限，用来拉取或推送映像或执行其他操作。 令牌提供的权限比其他注册表[身份验证选项](container-registry-authentication.md)更精细，后者指定的权限范围是整个注册表。 

适合创建令牌的场景包括：

* 允许具有单独令牌的 IoT 设备从存储库中拉取映像
* 向外部组织提供对特定存储库的权限 
* 针对组织中的不同用户组限制对存储库的访问权限。 例如，为面向特定存储库构建映像的开发人员提供写入和读取访问权限，为从这些存储库进行部署的团队提供读取访问权限。

> [!IMPORTANT]
> 此功能目前以预览版提供，存在一些[限制](#preview-limitations)。 需同意[补充使用条款][terms-of-use]才可使用预览版。 在正式版 (GA) 推出之前，此功能的某些方面可能会有所更改。

## <a name="preview-limitations"></a>预览版限制

* 此功能仅在“高级”容器注册表中可用。 有关注册表服务层级和限制的信息，请参阅 [Azure 容器注册表 SKU](container-registry-skus.md)。
* 当前无法将存储库范围的权限分配给 Azure Active Directory 标识，例如服务主体或托管标识。
* 无法在启用了[匿名拉取访问](container-registry-faq.md#how-do-i-enable-anonymous-pull-access)的注册表中创建范围映射。

## <a name="concepts"></a>概念

若要配置存储库范围的权限，请创建具有关联的“范围映射”的“令牌”。 

* 用户使用**令牌**与生成的密码通过注册表进行身份验证。 你可以为令牌密码设置到期日期，或者随时禁用令牌。  

    使用令牌进行身份验证后，用户或服务可以执行范围限定于一个或多个存储库的一个或多个操作。

    |操作  |说明  | 示例 |
    |---------|---------|--------|
    |`content/delete`    | 从存储库中删除数据  | 删除存储库或清单 |
    |`content/read`     |  从存储库中读取数据 |  拉取项目 |
    |`content/write`     |  将数据写入到存储库     | 与 `content/read` 一起使用来推送项目 |
    |`metadata/read`    | 从存储库中读取元数据   | 列出标记或清单 |
    |`metadata/write`     |  将元数据写入到存储库  | 启用或禁用读取、写入或删除操作 |

* **范围映射**会对你应用于令牌的存储库权限进行分组，并且可以重新应用于其他令牌。 每个令牌都与单个范围映射相关联。 

    使用范围映射可以执行以下操作：

    * 配置对一组存储库具有相同权限的多个令牌
    * 在范围映射中添加或删除存储库操作时更新令牌权限，或者在应用另一个范围映射时这样做 

    Azure 容器注册表还提供了多个系统定义的范围映射，你可以应用这些映射，它们在所有存储库中具有固定的权限。

下图显示了令牌与范围映射之间的关系。 

![注册表令牌与范围映射](media/container-registry-repository-scoped-permissions/token-scope-map-concepts.png)

## <a name="prerequisites"></a>先决条件

* **Azure CLI** - Azure CLI 2.0.76 或更高版本中提供了用于创建和管理令牌的 Azure CLI 命令。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。
* **Docker** - 若要通过注册表进行身份验证以拉取或推送映像，你需要一个本地 Docker 安装。 Docker 提供适用于 [macOS](https://docs.docker.com/docker-for-mac/)[Windows](https://docs.docker.com/docker-for-windows/) 和 [Linux](https://docs.docker.com/engine/installation/#supported-platforms) 系统的安装说明。
* **容器注册表** - 如果没有，请在你的 Azure 订阅中创建一个高级容器注册表，或升级现有注册表。 例如，使用 [Azure 门户](container-registry-get-started-portal.md)或 [Azure CLI](container-registry-get-started-azure-cli.md)。 

## <a name="create-token---cli"></a>创建令牌 - CLI

### <a name="create-token-and-specify-repositories"></a>创建令牌并指定存储库

可使用 [az acr token create][az-acr-token-create] 命令创建令牌。 创建令牌时，可以指定一个或多个存储库，并在每个存储库上指定关联的操作。 存储库不需要已在注册表中。 若要通过指定现有范围映射来创建令牌，请参阅下一部分。

下面的示例在注册表 myregistry 中创建一个令牌，该令牌对 `samples/hello-world` 存储库具有以下权限：`content/write` 和 `content/read`。 默认情况下，该命令会将默认令牌状态设置为 `enabled`，但你随时可以将状态更新为 `disabled`。

```azurecli
az acr token create --name MyToken --registry myregistry \
  --repository samples/hello-world \
  content/write content/read
```

输出会显示有关令牌的详细信息，包括生成的两个密码。 建议将密码保存在安全的位置，以便以后将其用于身份验证。 无法再次检索这些密码，但可以生成新密码。

```console
{
  "creationDate": "2020-01-18T00:15:34.066221+00:00",
  "credentials": {
    "certificates": [],
    "passwords": [
      {
        "creationTime": "2020-01-18T00:15:52.837651+00:00",
        "expiry": null,
        "name": "password1",
        "value": "uH54BxxxxK7KOxxxxRbr26dAs8JXxxxx"
      },
      {
        "creationTime": "2020-01-18T00:15:52.837651+00:00",
        "expiry": null,
        "name": "password2",
        "value": "kPX6Or/xxxxLXpqowxxxxkA0idwLtmxxxx"
      }
    ],
    "username": "MyToken"
  },
  "id": "/subscriptions/xxxxxxxx-adbd-4cb4-c864-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry/tokens/MyToken",
  "name": "MyToken",
  "objectId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "scopeMapId": "/subscriptions/xxxxxxxx-adbd-4cb4-c864-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry/scopeMaps/MyToken-scope-map",
  "status": "enabled",
  "type": "Microsoft.ContainerRegistry/registries/tokens"
```

输出包含该命令创建的范围映射的详细信息。 可以使用范围映射（在此处名为 `MyToken-scope-map`）将相同的存储库操作应用于其他令牌。 或者，稍后更新范围映射以更改关联的令牌的权限。

### <a name="create-token-and-specify-scope-map"></a>创建令牌并指定范围映射

创建令牌的另一种方法是指定现有的范围映射。 如果还没有范围映射，请先通过指定存储库和关联的操作来创建一个。 然后，在创建令牌时指定该范围映射。 

若要创建范围映射，请使用 [az acr scope-map create][az-acr-scope-map-create] 命令。 以下命令创建的范围映射对前面使用的 `samples/hello-world` 存储库具有相同权限。 

```azurecli
az acr scope-map create --name MyScopeMap --registry myregistry \
  --repository samples/hello-world \
  content/write content/read \
  --description "Sample scope map"
```

运行 [az acr token create][az-acr-token-create] 来创建令牌，同时指定 *MyScopeMap* 范围映射。 与前面的示例一样，该命令会将默认令牌状态设置为 `enabled`。

```azurecli
az acr token create --name MyToken \
  --registry myregistry \
  --scope-map MyScopeMap
```

输出会显示有关令牌的详细信息，包括生成的两个密码。 建议将密码保存在安全的位置，以便以后将其用于身份验证。 无法再次检索这些密码，但可以生成新密码。

<!--Not Available on ## Create token - portal-->

## <a name="authenticate-with-token"></a>使用令牌进行身份验证

用户或服务在使用令牌通过目标注册表进行身份验证时，会提供令牌名称作为用户名并提供其生成的密码之一。 身份验证方法取决于所配置的与令牌关联的一个或多个操作。

|操作  |如何进行身份验证  |
|---------|---------|
|`content/delete`    | Azure CLI 中的 `az acr repository delete` |
|`content/read`     |  `docker login`<br/><br/>Azure CLI 中的 `az acr login`  |
|`content/write`     |  `docker login`<br/><br/>Azure CLI 中的 `az acr login`     |
|`metadata/read`    | `az acr repository show`<br/><br/>`az acr repository show-tags`<br/><br/>Azure CLI 中的 `az acr repository show-manifests`   |
|`metadata/write`     |  `az acr repository untag`<br/><br/>Azure CLI 中的 `az acr repository update` |

## <a name="examples-use-token"></a>示例:使用令牌

以下示例使用本文前面创建的令牌对存储库执行常见操作：推送和拉取映像、删除映像以及列出存储库标记。 最初为该令牌设置了对 `samples/hello-world` 存储库的推送权限（`content/write` 和 `content/read` 操作）。

### <a name="pull-and-tag-test-images"></a>拉取和标记测试映像

以下示例从 Docker Hub 拉取 `hello-world` 和 `alpine` 映像，并针对你的注册表和存储库标记它们。

```bash
docker pull hello-world
docker pull alpine
docker tag hello-world myregistry.azurecr.cn/samples/hello-world:v1
docker tag hello-world myregistry.azurecr.cn/samples/alpine:v1
```

### <a name="authenticate-using-token"></a>使用令牌进行身份验证

运行 `docker login` 以通过注册表进行身份验证，提供令牌名称作为用户名，并提供其密码之一。 令牌必须具有 `Enabled` 状态。

以下示例针对 bash shell 进行了格式设置，并使用环境变量提供值。

```bash
TOKEN_NAME=MyToken
TOKEN_PWD=<token password>

echo $TOKEN_PWD | docker login --username $TOKEN_NAME --password-stdin myregistry.azurecr.cn
```

输出应显示身份验证成功：

```console
Login Succeeded
```

### <a name="push-images-to-registry"></a>将映像推送到注册表

成功登录后，尝试将标记的映像推送到注册表。 由于该令牌有权将映像推送到 `samples/hello-world` 存储库，因此以下推送会成功：

```bash
docker push myregistry.azurecr.cn/samples/hello-world:v1
```

该令牌无权访问 `samples/alpine` 存储库，因此，以下推送尝试会失败，并出现类似于“`requested access to the resource is denied`”的错误：

```bash
docker push myregistry.azurecr.cn/samples/alpine:v1
```

### <a name="change-pushpull-permissions"></a>更改推送/拉取权限

若要更新令牌的权限，请更新关联的范围映射中的权限。 更新的范围映射会立即应用于所有关联的令牌。 

例如，使用对 `samples/alpine` 存储库的 `content/write` 和 `content/read` 操作更新 `MyToken-scope-map`，并删除对 `samples/hello-world` 存储库的 `content/write` 操作。  

若要使用 Azure CLI，请运行 [az acr scope-map update][az-acr-scope-map-update] 来更新范围映射：

```azurecli
az acr scope-map update \
  --name MyScopeMap \
  --registry myregistry \
  --add samples/alpine content/write content/read \
  --remove samples/hello-world content/write 
```

<!--Not Available on In the Azure portal:-->

更新范围映射后，以下推送会成功：

```bash
docker push myregistry.azurecr.cn/samples/alpine:v1
```

由于范围映射对 `samples/hello-world` 存储库仅具有 `content/read` 权限，因此，尝试推送到 `samples/hello-world` 存储库现在会失败：
 
```bash
docker push myregistry.azurecr.cn/samples/hello-world:v1
```

从这两个存储库中拉取映像都会成功，因为范围映射提供了对这两个存储库的 `content/read` 权限：

```bash
docker pull myregistry.azurecr.cn/samples/alpine:v1
docker pull myregistry.azurecr.cn/samples/hello-world:v1
```
### <a name="delete-images"></a>删除映像

通过添加对 `alpine` 存储库的 `content/delete` 操作来更新范围映射。 此操作允许删除存储库中的映像或删除整个存储库。

为简洁起见，我们仅显示用来更新范围映射的 [az acr scope-map update][az-acr-scope-map-update] 命令：

```azurecli
az acr scope-map update \
  --name MyScopeMap \
  --registry myregistry \
  --add samples/alpine content/delete
``` 

<!--Not Available on To update the scope map using the portal, see the preceding section.-->

可使用以下 [az acr repository delete][az-acr-repository-delete] 命令删除 `samples/alpine` 存储库。 若要删除映像或存储库，不能通过 `docker login` 对令牌进行身份验证， 而是需要将令牌的名称和密码传递给命令。 以下示例使用本文前面创建的环境变量：

```azurecli
az acr repository delete \
  --name myregistry --repository samples/alpine \
  --username $TOKEN_NAME --password $TOKEN_PWD
```

### <a name="show-repo-tags"></a>显示存储库标记 

通过添加对 `hello-world` 存储库的 `metadata/read` 操作来更新范围映射。 此操作允许读取存储库中的清单和标记数据。

为简洁起见，我们仅显示用来更新范围映射的 [az acr scope-map update][az-acr-scope-map-update] 命令：

```azurecli
az acr scope-map update \
  --name MyScopeMap \
  --registry myregistry \
  --add samples/hello-world metadata/read 
```  

<!--Not Available on To update the scope map using the portal, see the preceding section.-->

若要读取 `samples/hello-world` 存储库中的元数据，请运行 [az acr repository show-manifests][az-acr-repository-show-manifests] 或 [az acr repository show-tags][az-acr-repository-show-tags] 命令。 

若要读取元数据，不能通过 `docker login` 对令牌进行身份验证， 而是需要将令牌的名称和密码传递给任一命令。 以下示例使用本文前面创建的环境变量：

```azurecli
az acr repository show-tags \
  --name myregistry --repository samples/hello-world \
  --username $TOKEN_NAME --password $TOKEN_PWD
```

示例输出：

```console
[
  "v1"
]
```
## <a name="manage-tokens-and-scope-maps"></a>管理令牌与范围映射

### <a name="list-scope-maps"></a>列出范围映射

可以使用 [az acr scope-map list][az-acr-scope-map-list] 命令列出在注册表中配置的所有范围映射。 例如：

<!--Not Available on  or the **Scope maps (Preview)** screen in the portal-->

```azurecli
az acr scope-map list \
  --registry myregistry --output table
```

输出会显示你定义的范围映射，此外还会显示多个系统定义的供你用来配置令牌的范围映射：

```
NAME                 TYPE           CREATION DATE         DESCRIPTION
-------------------  -------------  --------------------  ------------------------------------------------------------
_repositories_admin  SystemDefined  2020-01-20T09:44:24Z  Can perform all read, write and delete operations on the ...
_repositories_pull   SystemDefined  2020-01-20T09:44:24Z  Can pull any repository of the registry
_repositories_push   SystemDefined  2020-01-20T09:44:24Z  Can push to any repository of the registry
MyScopeMap           UserDefined    2019-11-15T21:17:34Z  Sample scope map
```

### <a name="show-token-details"></a>显示令牌详细信息

若要查看令牌的详细信息（例如其状态和密码到期日期），请运行 [az acr token show][az-acr-token-show] 命令。 例如：

<!--Not Available on or select the token in the **Tokens (Preview)** screen in the portal.-->

```azurecli
az acr scope-map show \
  --name MyScopeMap --registry myregistry
```

可以使用 [az acr token list][az-acr-token-list] 命令列出在注册表中配置的所有令牌。 例如：

<!--Not Available on  or the **Tokens (Preview)** screen in the portal, -->

```azurecli
az acr token list --registry myregistry --output table
```

### <a name="generate-passwords-for-token"></a>为令牌生成密码

如果没有令牌密码，或者要生成新密码，请运行 [az acr token credential generate][az-acr-token-credential-generate] 命令。 

以下示例为 *MyToken* 令牌的 password1 生成新值，有效期为 30 天。 它将密码存储在环境变量 `TOKEN_PWD` 中。 此示例的格式是针对 Bash shell 设置的。

```azurecli
TOKEN_PWD=$(az acr token credential generate \
  --name MyToken --registry myregistry --days 30 \
  --password1 --query 'passwords[0].value' --output tsv)
```

<!--NOt Available on To use the Azure portal to generate a token password, see the steps in [Create token - portal](#create-token---portal) earlier in this article.-->

### <a name="update-token-with-new-scope-map"></a>使用新的范围映射更新令牌

如果要使用一个不同的范围映射来更新令牌，请运行 [az acr token update][az-acr-token-update] 并指定新的范围映射。 例如：

```azurecli
az acr token update --name MyToken --registry myregistry \
  --scope-map MyNewScopeMap
```

<!--NOt Available on In the portal, on the **Tokens (preview)** screen, select the token, and under **Scope map**, select a different scope map.-->

> [!TIP]
> 使用新的范围映射更新令牌后，可能需要生成新的令牌密码。 请使用 [az acr token credential generate][az-acr-token-credential-generate] 命令。

<!--Not Available on or regenerate a token password in the Azure portal.-->

## <a name="disable-or-delete-token"></a>禁用或删除令牌

可能需要临时对用户或服务禁用令牌凭据。 

使用 Azure CLI 运行 [az acr token update][az-acr-token-update] 命令，将 `status` 设置为 `disabled`：

```azurecli
az acr token update --name MyToken --registry myregistry \
  --status disabled
```

<!--Not Available on In the portal, select the token in the **Tokens (Preview)** screen, and select **Disabled** under **Status**.-->

若要删除令牌以使利用其凭据的任何人的访问权限永久失效，请运行 [az acr token delete][az-acr-token-delete] 命令。 

```azurecli
az acr token delete --name MyToken --registry myregistry
```

<!--Not Available on In the portal, select the token in the **Tokens (Preview)** screen, and select **Discard**.-->

## <a name="next-steps"></a>后续步骤

* 若要管理范围映射和令牌，请使用 [az acr scope-map][az-acr-scope-map] 和 [az acr token][az-acr-token] 命令组中的其他命令。
* 请参阅[身份验证概述](container-registry-authentication.md)，以了解通过 Azure 容器注册表进行身份验证的其他选项，包括使用 Azure Active Directory 标识、服务主体或管理员帐户。

<!-- LINKS - External -->

[terms-of-use]: https://www.azure.cn/support/legal/subscription-agreement/

<!-- LINKS - Internal -->

[az-acr-login]: https://docs.azure.cn/cli/acr?view=azure-cli-latest#az-acr-login
[az-acr-repository]: https://docs.azure.cn/cli/acr/repository/?view=azure-cli-latest
[az-acr-repository-show-tags]: https://docs.azure.cn/cli/acr/repository/?view=azure-cli-latest#az-acr-repository-show-tags
[az-acr-repository-show-manifests]: https://docs.azure.cn/cli/acr/repository/?view=azure-cli-latest#az-acr-repository-show-manifests
[az-acr-repository-delete]: https://docs.azure.cn/cli/acr/repository/?view=azure-cli-latest#az-acr-repository-delete
[az-acr-scope-map]: https://docs.microsoft.com/cli/azure/acr/scope-map/?view=azure-cli-latest
[az-acr-scope-map-create]: https://docs.microsoft.com/cli/azure/acr/scope-map/?view=azure-cli-latest#az-acr-scope-map-create
[az-acr-scope-map-list]: https://docs.microsoft.com/cli/azure/acr/scope-map/?view=azure-cli-latest#az-acr-scope-map-show
[az-acr-scope-map-show]: https://docs.microsoft.com/cli/azure/acr/scope-map/?view=azure-cli-latest#az-acr-scope-map-list
[az-acr-scope-map-update]: https://docs.microsoft.com/cli/azure/acr/scope-map/?view=azure-cli-latest#az-acr-scope-map-update
[az-acr-scope-map-list]: https://docs.microsoft.com/cli/azure/acr/scope-map/?view=azure-cli-latest#az-acr-scope-map-list
[az-acr-token]: https://docs.microsoft.com/cli/azure/acr/token/?view=azure-cli-latest
[az-acr-token-show]: https://docs.microsoft.com/cli/azure/acr/token/?view=azure-cli-latest#az-acr-token-show
[az-acr-token-list]: https://docs.microsoft.com/cli/azure/acr/token/?view=azure-cli-latest#az-acr-token-list
[az-acr-token-delete]: https://docs.microsoft.com/cli/azure/acr/token/?view=azure-cli-latest#az-acr-token-delete
[az-acr-token-create]: https://docs.microsoft.com/cli/azure/acr/token/?view=azure-cli-latest#az-acr-token-create
[az-acr-token-update]: https://docs.microsoft.com/cli/azure/acr/token/?view=azure-cli-latest#az-acr-token-update
[az-acr-token-credential-generate]: https://docs.microsoft.com/cli/azure/acr/token/credential/?view=azure-cli-latest#az-acr-token-credential-generate


<!-- Update_Description: new article about container registry repository scoped permissions -->
<!--NEW.date: 04/30/2020-->