---
title: Azure 事件网格 - 为主题启用诊断日志
description: 本文提供了有关如何为 Azure 事件网格主题启用诊断日志的分步说明。
services: event-grid
author: lingliw
ms.service: event-grid
ms.topic: how-to
origin.date: 01/30/2020
ms.date: 02/17/2020
ms.author: v-lingwu
ms.openlocfilehash: 490e793cacc047145d754fd863aefc78af394d0f
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79452588"
---
#  <a name="diagnostic-logs-for-an-azure-event-grid-topic"></a>Azure 事件网格主题的诊断日志
诊断设置允许事件网格用户在以下位置之一捕获和查看发布和传递失败日志：Azure存储帐户、事件中心或 Log Analytics 工作区。 本文提供了有关如何为事件网格主题启用诊断日志的分步说明。

## <a name="prerequisites"></a>必备条件

- 预配的事件网格主题
- 用于捕获诊断日志的预配目标。 这可以是以下目标之一：
    - Azure 存储帐户
    - 事件中心
    - Log Analytics 工作区


## <a name="steps-for-enabling-diagnostic-logs-for-a-topic"></a>为主题启用诊断日志的步骤

1. 登录 [Azure 门户](https://portal.azure.cn)。
2. 导航到要为其启用诊断日志设置的事件网格主题。 
3. 在左侧菜单中的“监视”下，选择“诊断设置”   。
4. 在“诊断设置”页面上，选择“添加新的诊断设置”。   
    
    ![“添加诊断设置”按钮](./media/enable-diagnostic-logs-topic/diagnostic-settings-add.png)
5. 为诊断设置指定一个名称。  

    ![诊断设置 - 名称](./media/enable-diagnostic-logs-topic/diagnostic-settings-name.png)     
6. 为日志启用一个或多个捕获目标，然后通过选择以前创建的捕获资源对其进行配置。 
    - 如果选择“存档到存储帐户”  ，请选择“存储帐户 - 配置”  ，然后选择你的 Azure 订阅中的存储帐户。 

        ![存档到 Azure 存储帐户](./media/enable-diagnostic-logs-topic/archive-storage.png)
    - 如果选择“流式传输到事件中心”  ，请选择“事件中心 - 配置”  ，然后选择事件中心命名空间、事件中心和访问策略。 
        ![流式传输到事件中心](./media/enable-diagnostic-logs-topic/archive-event-hub.png)
    - 如果选择“发送到 Log Analytics”  ，请选择 Log Analytics 工作区。
        ![发送到 Log Analytics](./media/enable-diagnostic-logs-topic/send-log-analytics.png)
7. 在“日志”  部分中选择 **DeliveryFailures** 和 **PublishFailures** 选项。 
    ![选择具体的失败](./media/enable-diagnostic-logs-topic/log-failures.png)
8. 选择“保存”。  选择右上角的 **X** 以关闭页面。 
9. 现在，返回“诊断设置”  页面，确认在“诊断设置”  表中看到了一个新条目。 
    ![列表中的诊断设置](./media/enable-diagnostic-logs-topic/diagnostic-setting-list.png)

     你还可以为主题启用所有指标的收集。 

