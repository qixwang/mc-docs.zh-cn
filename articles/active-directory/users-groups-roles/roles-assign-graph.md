---
title: 向 Microsoft Graph API 分配 Azure AD 管理员角色 |Microsoft Docs
description: 在 Azure Active Directory 中使用图形 API 分配和移除 Azure AD 管理员角色
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: how-to
ms.date: 07/06/2020
ms.author: v-junlch
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3c24046d4755501f3a9d1291d6b32be40cf05de7
ms.sourcegitcommit: 92b9b1387314b60661f5f62db4451c9ff2c49500
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2020
ms.locfileid: "86165029"
---
# <a name="assign-custom-admin-roles-using-the-microsoft-graph-api-in-azure-active-directory"></a>在 Azure Active Directory 中使用 Microsoft 图形 API 分配自定义管理员角色 

你可以使用 Microsoft Graph API 自动执行将角色分配给用户帐户的过程。 本文介绍了 roleAssignment 上的 POST、GET 和 DELETE 操作。

## <a name="required-permissions"></a>所需的权限

使用全局管理员帐户或特权标识管理员连接到 Azure AD 组织，以分配或移除角色。

## <a name="post-operations-on-roleassignment"></a>RoleAssignment 上的 POST 操作

用于在用户和角色定义之间创建角色分配的 HTTP 请求。

POST

``` HTTP
POST https://microsoftgraph.chinacloudapi.cn/beta/roleManagement/directory/roleAssignments
Content-type: application/json
```

Body

``` HTTP
{
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"194ae4cb-b126-40b2-bd5b-6091b380977d",
    "resourceScopes":"/"
}
```

响应

``` HTTP
HTTP/1.1 201 Created
```

用于在不存在主体或角色定义的情况下创建角色分配的 HTTP 请求

POST

``` HTTP
https://microsoftgraph.chinacloudapi.cn/beta/roleManagement/directory/roleAssignments
```

Body

``` HTTP
{
    "principalId":" 2142743c-a5b3-4983-8486-4532ccba12869",
    "roleDefinitionId":"194ae4cb-b126-40b2-bd5b-6091b380977d",
    "resourceScopes":"/"
}
```

响应

``` HTTP
HTTP/1.1 404 Not Found
```

用于在内置角色定义上创建单个资源范围内的角色分配的 HTTP 请求。

> [!NOTE] 
> 目前，内置角色的限制仅限于“/”组织范围范围或“/AU/*”范围。 单个资源范围不适用于内置角色，但适用于自定义角色。

POST

``` HTTP
https://microsoftgraph.chinacloudapi.cn/beta/roleManagement/directory/roleAssignments
```

Body

``` HTTP
{
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"194ae4cb-b126-40b2-bd5b-6091b380977d",
    "resourceScopes":"/ab2e1023-bddc-4038-9ac1-ad4843e7e539"
}
```

响应

``` HTTP
HTTP/1.1 400 Bad Request
{
    "odata.error":
    {
        "code":"Request_BadRequest",
        "message":
        {
            "lang":"en",
            "value":"Provided authorization scope is not supported for built-in role definitions."},
            "values":
            [
                {
                    "item":"scope",
                    "value":"/ab2e1023-bddc-4038-9ac1-ad4843e7e539"
                }
            ]
        }
    }
}
```

## <a name="get-operations-on-roleassignment"></a>RoleAssignment 上的 GET 操作

用于为给定主体获取角色分配的 HTTP 请求

GET

``` HTTP
https://microsoftgraph.chinacloudapi.cn/beta/roleManagement/directory/roleAssignments&$filter=principalId eq ‘<object-id-of-principal>’
```

响应

``` HTTP
HTTP/1.1 200 OK
{ 
    "id":"mhxJMipY4UanIzy2yE-r7JIiSDKQoTVJrLE9etXyrY0-1"
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"10dae51f-b6af-4016-8d66-8c2a99b929b3",
    "resourceScopes":"/"
} ,
{
    "id":"CtRxNqwabEKgwaOCHr2CGJIiSDKQoTVJrLE9etXyrY0-1"
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"3671d40a-1aac-426c-a0c1-a3821ebd8218",
    "resourceScopes":"/"
}
```

用于为给定角色定义获取角色分配的 HTTP 请求。

GET

``` HTTP
https://microsoftgraph.chinacloudapi.cn/beta/roleManagement/directory/roleAssignments&$filter=roleDefinitionId eq ‘<object-id-or-template-id-of-role-definition>’
```

响应

``` HTTP
HTTP/1.1 200 OK
{
    "id":"CtRxNqwabEKgwaOCHr2CGJIiSDKQoTVJrLE9etXyrY0-1"
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"3671d40a-1aac-426c-a0c1-a3821ebd8218",
    "resourceScopes":"/"
}
```

用于按 ID 获取角色分配的 HTTP 请求。

GET

``` HTTP
GET https://microsoftgraph.chinacloudapi.cn/beta/roleManagement/directory/roleAssignments/lAPpYvVpN0KRkAEhdxReEJC2sEqbR_9Hr48lds9SGHI-1
```

响应

``` HTTP
HTTP/1.1 200 OK
{ 
    "id":"mhxJMipY4UanIzy2yE-r7JIiSDKQoTVJrLE9etXyrY0-1",
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"10dae51f-b6af-4016-8d66-8c2a99b929b3",
    "resourceScopes":"/"
}
```

## <a name="delete-operations-on-roleassignment"></a>RoleAssignment 上的 DELETE 操作

用于删除用户和角色定义之间的角色分配的 HTTP 请求。

DELETE

``` HTTP
GET https://microsoftgraph.chinacloudapi.cn/beta/roleManagement/directory/roleAssignments/lAPpYvVpN0KRkAEhdxReEJC2sEqbR_9Hr48lds9SGHI-1
```

响应
``` HTTP
HTTP/1.1 204 No Content
```

用于删除不再存在的角色分配的 HTTP 请求

DELETE

``` HTTP
GET https://microsoftgraph.chinacloudapi.cn/beta/roleManagement/directory/roleAssignments/lAPpYvVpN0KRkAEhdxReEJC2sEqbR_9Hr48lds9SGHI-1
```

响应

``` HTTP
HTTP/1.1 404 Not Found
```

用于删除自己和内置角色定义之间的角色分配的 HTTP 请求

DELETE

``` HTTP
GET https://microsoftgraph.chinacloudapi.cn/beta/roleManagement/directory/roleAssignments/lAPpYvVpN0KRkAEhdxReEJC2sEqbR_9Hr48lds9SGHI-1
```

响应

``` HTTP
HTTP/1.1 400 Bad Request
{
    "odata.error":
    {
        "code":"Request_BadRequest",
        "message":
        {
            "lang":"en",
            "value":"Cannot remove self from built-in role definitions."},
            "values":null
        }
    }
}
```

## <a name="next-steps"></a>后续步骤

* 欢迎在 [Azure AD 管理角色论坛](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032)上与我们分享知识。
* 有关角色以及管理员角色分配的详细信息，请参阅[分配管理员角色](directory-assign-admin-roles.md)。
* 有关默认用户权限，请参阅[默认来宾和成员用户权限的比较](../fundamentals/users-default-permissions.md)。

