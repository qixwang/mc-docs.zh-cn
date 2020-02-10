---
title: 使用 Azure Cosmos DB 的用于 MongoDB 的 API 查询数据
description: 了解如何使用 MongoDB shell 命令从 Azure Cosmos DB 的 API for MongoDB 查询数据
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: tutorial
origin.date: 12/03/2019
ms.date: 02/10/2020
ms.reviewer: sngun
ms.openlocfilehash: f3422f80758a229f281d5240a5ab12982d9fe5e4
ms.sourcegitcommit: 925c2a0f6c9193c67046b0e67628d15eec5205c3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77068334"
---
# <a name="query-data-by-using-azure-cosmos-dbs-api-for-mongodb"></a>使用 Azure Cosmos DB 的用于 MongoDB 的 API 查询数据

[Azure Cosmos DB 的用于 MongoDB 的 API](mongodb-introduction.md) 支持 [MongoDB 查询](https://docs.mongodb.com/manual/tutorial/query-documents/)。 

本文涵盖以下任务： 

> [!div class="checklist"]
> * 使用 MongoDB shell 查询 Cosmos 数据库中存储的数据

可以使用本文档中的示例来快速入门。

<!--Not Available on  and watch the [Query Azure Cosmos DB with MongoDB shell](https://www.azure.cn/resources/videos/query-azure-cosmos-db-data-by-using-the-mongodb-shell/)-->

## <a name="sample-document"></a>示例文档

本文中的查询使用下面的示例文档。

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```
<a name="examplequery1"></a>
## <a name="example-query-1"></a>示例查询 1 

在上述家庭示例文档中，以下查询返回其 ID 字段匹配 `WakefieldFamily` 的文档。

**查询**

    db.families.find({ id: "WakefieldFamily"})

**结果**

    {
    "_id": "ObjectId(\"58f65e1198f3a12c7090e68c\")",
    "id": "WakefieldFamily",
    "parents": [
      {
        "familyName": "Wakefield",
        "givenName": "Robin"
      },
      {
        "familyName": "Miller",
        "givenName": "Ben"
      }
    ],
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ],
    "address": {
      "state": "NY",
      "county": "Manhattan",
      "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
    }

<a name="examplequery2"></a>
## <a name="example-query-2"></a>示例查询 2 

下一个查询返回该家庭中的所有子女。 

**查询**

    db.families.find( { id: "WakefieldFamily" }, { children: true } )

**结果**

    {
    "_id": "ObjectId("58f65e1198f3a12c7090e68c")",
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ]
    }

<a name="examplequery3"></a>
## <a name="example-query-3"></a>示例查询 3 

下一个查询返回所有已注册的家庭。 

**查询**

    db.families.find( { "isRegistered" : true })
**结果**不返回任何文档。 

<a name="examplequery4"></a>
## <a name="example-query-4"></a>示例查询 4

下一个查询返回所有未注册的家庭。 

**查询**

    db.families.find( { "isRegistered" : false })
**结果**

    {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
    }

<a name="examplequery5"></a>
## <a name="example-query-5"></a>示例查询 5

下一个查询返回所有未注册且所在州为纽约 (NY) 的家庭。 

**查询**

    db.families.find( { "isRegistered" : false, "address.state" : "NY" })

**结果**

    {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
    }

<a name="examplequery6"></a>
## <a name="example-query-6"></a>示例查询 6

下一个查询返回子女读 8 年级的所有家庭。

**查询**

     db.families.find( { children : { $elemMatch: { grade : 8 }} } )

**结果**

    {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
    }

<a name="examplequery7"></a>
## <a name="example-query-7"></a>示例查询 7

下一个查询返回有 3 个子女的家庭。

**查询**

    db.Family.find( {children: { $size:3} } )

**结果**

将不返回任何结果，因为没有任何家庭的子女数超过 2。 仅当参数为 2 时此查询才能成功，并返回完整的文档。

## <a name="next-steps"></a>后续步骤

在本教程中已完成以下操作：

> [!div class="checklist"]
> * 学习了如何使用 Cosmos DB 的用于 MongoDB 的 API 进行查询

现可继续学习下一教程，了解如何多区域分发数据。

> [!div class="nextstepaction"]
> [多区域分配数据](tutorial-global-distribution-sql-api.md)

<!-- Update_Description: update meta properties, wording update -->