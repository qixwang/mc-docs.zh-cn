---
title: 限制和配置
description: Azure 逻辑应用的服务限制（例如持续时间、吞吐量和容量）以及配置值（例如允许的 IP 地址）
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
origin.date: 03/12/2020
ms.date: 03/30/2020
ms.author: v-yeche
ms.openlocfilehash: 8f9e764b877da2c6f1444cc18df5735f34ffae6f
ms.sourcegitcommit: be0a8e909fbce6b1b09699a721268f2fc7eb89de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84199715"
---
# <a name="limits-and-configuration-information-for-azure-logic-apps"></a>Azure 逻辑应用的限制和配置信息

本文介绍使用 Azure 逻辑应用创建和运行自动化工作流的限制和配置的详细信息。 对于 Power Automate，请参阅 [Power Automate 中的限制和配置](https://docs.microsoft.com/flow/limits-and-config)。

<a name="definition-limits"></a>

## <a name="definition-limits"></a>定义限制

下面是针对单个逻辑应用定义的限制：

| 名称 | 限制 | 注释 |
| ---- | ----- | ----- |
| 每个工作流的操作数 | 500 | 要对此限制进行扩展，可根据需要添加嵌套工作流。 |
| 操作的允许嵌套深度 | 8 | 要对此限制进行扩展，可根据需要添加嵌套工作流。 |
| 每个订阅每个区域的工作流数 | 1,000 | |
| 每个工作流的触发数 | 10 个 | 在代码视图中工作时，而不是在设计器中工作时 |
| Switch 作用域事例限制 | 25 | |
| 每个工作流的变量数 | 250 | |
| 每个表达式的字符数 | 8,192 | |
| `trackedProperties` 的最大大小 | 16,000 个字符 |
| `action` 或 `trigger` 的名称 | 80 个字符 | |
| `description` 的长度 | 256 个字符 | |
| `parameters` 最大值 | 50 | |
| `outputs` 最大值 | 10 个 | |
||||

<a name="run-duration-retention-limits"></a>

## <a name="run-duration-and-retention-limits"></a>运行持续时间和保留期限制

下面是针对单个逻辑应用运行的限制：

<!--Not Available on Integration service environment limit-->

| 名称 | 多租户限制 |  注释 |
|------|--------------------|-------|
| 运行持续时间 | 90 天 |  运行持续时间是使用运行开始时间以及在开始时由工作流设置“[**运行历史记录保留期(天)** ](#change-duration)”指定的限制计算的。  <p><p>若要更改默认限制（90 天），请参阅[更改运行持续时间](#change-duration)。 |
| 存储中的运行保留期 | 90 天 | 运行保留期是使用运行开始时间以及在当前时间由工作流设置“[**运行历史记录保留期(天)** ](#change-retention)”指定的限制计算的。  无论运行是完成还是超时，保留期计算始终使用运行开始时间。 当运行的持续时间超过当前保留期限制时，将从运行历史记录中删除该运行。  <p><p>如果更改此设置，将始终使用当前限制来计算保留期，而不考虑以前的限制。 例如，如果将保留期限制从 90 天减至 30 天，则会从运行历史记录中删除保留了 60 天的运行。 如果将保留期从 30 天增至 60 天，则已经保留了 20 天之久的运行将在运行历史记录中继续保留 40 天。 <p><p>若要更改默认限制（90 天），请参阅[更改存储中的运行保留期](#change-retention)。 |
| 最小重复间隔 | 1 秒 | |
| 最大重复间隔 | 500 天 | |
||||

<a name="change-duration"></a>
<a name="change-retention"></a>

### <a name="change-run-duration-and-run-retention-in-storage"></a>更改存储中的运行持续时间和运行保留期

若要更改存储中的运行持续时间和运行保留期的默认限制，请执行以下步骤。 若要提高最大限制，[请联系逻辑应用团队](mailto://logicappsemail@microsoft.com)，就你的要求获取帮助。

> [!NOTE]
> 对于多租户 Azure 中的逻辑应用，90 天默认限制与最大限制相同。 只能减小此值。
> 对于集成服务环境中的逻辑应用，可以降低或提高 90 天默认限制。

1. 转到 [Azure 门户](https://portal.azure.cn)。 在门户搜索框中，找到并选择“逻辑应用”。 

1. 在逻辑应用设计器中选择并打开你的逻辑应用。

1. 在逻辑应用的菜单中选择“工作流设置”。 

1. 在“运行时选项”下，从“运行历史记录保留天数”列表中选择“自定义”    。

1. 拖动滑块更改所需的天数。

1. 完成后，在“工作流设置”工具栏上选择“保存”。  

<a name="looping-debatching-limits"></a>

## <a name="concurrency-looping-and-debatching-limits"></a>并发、循环和拆分限制

下面是针对单个逻辑应用运行的限制：

| 名称 | 限制 | 注释 |
| ---- | ----- | ----- |
| 触发器并发 | - 在禁用并发控制时无限制 <p><p>- 在启用并发控制时，25 是默认限制，在启用控制后无法取消此限制。 可以将默认值更改为介于 1 与 50（含）之间的值。 | 此限制描述可以在同一时间或并行运行的逻辑应用实例的最大数。 <p><p>**注意**：启用并发后，[解除数组批处理](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch)的 SplitOn 限制会降低到 100 个项。 <p><p>若要将默认限制更改为介于 1 到 50 之间（含）的值，请参阅[更改触发器并发限制](../logic-apps/logic-apps-workflow-actions-triggers.md#change-trigger-concurrency)或[按顺序触发实例](../logic-apps/logic-apps-workflow-actions-triggers.md#sequential-trigger)。 |
| 最大等待运行数 | - 未启用并发时，最小等待运行数是 1，最大数目是 50。 <p><p>- 启用并发时，最小等待运行数是 10 加上并发运行（触发器并发）数。 可以将最大数更改为多达 100 个（含）。 | 此限制描述当逻辑应用已在运行最大数量并发实例时，可等待运行的最大逻辑应用实例数。 <p><p>若要更改此默认限制，请参阅[更改等待的运行限制](../logic-apps/logic-apps-workflow-actions-triggers.md#change-waiting-runs)。 |
| Foreach 数组项 | 100,000 | 此限制描述“for each”循环可以处理的最大数组项数。 <p><p>可以使用[查询操作](logic-apps-perform-data-operations.md#filter-array-action)筛选更大数组。 |
| Foreach 并发 | 20 是并发控制关闭时的默认限制。 可以将默认值更改为介于 1 与 50（含）之间的值。 | 此限制是可同时或并行运行的最大“for each”循环迭代数。 <p><p>若要将默认限制更改为介于 1 到 50 之间（含）的值，请参阅[更改“for each”并发限制](../logic-apps/logic-apps-workflow-actions-triggers.md#change-for-each-concurrency)或[按顺序运行“for each”循环](../logic-apps/logic-apps-workflow-actions-triggers.md#sequential-for-each)。 |
| SplitOn 项 | - 未启用触发器并发时为 100,000 <p><p>- 启用触发器并发时为 100 | 对于返回数组的触发器，可指定一个表达式，它使用[将数组项拆分或解除批处理到多个工作流实例](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch)进行处理的“SplitOn”属性，而不是使用“Foreach”循环。 此表达式引用要用于为每个数组项创建和运行工作流实例的数组。 <p><p>**注意**：启用并发后，SplitOn 限制会降低到 100 个项。 |
| Until 迭代 | - 默认值：60 <p><p>- 最大值：5,000 | |
||||

<a name="throughput-limits"></a>

## <a name="throughput-limits"></a>吞吐量限制

下面是针对单个逻辑应用定义的限制：

### <a name="multi-tenant-logic-apps-service"></a>多租户逻辑应用服务

| 名称 | 限制 | 注释 |
| ---- | ----- | ----- |
| 操作：每 5 分钟执行的次数 | 默认限制为 100,000，最大限制为 300,000。 | 若要更改此默认限制，请参阅处于预览阶段的[在“高吞吐量”模式下运行逻辑应用](../logic-apps/logic-apps-workflow-actions-triggers.md#run-high-throughput-mode)。 或者，你可根据需要在多个逻辑应用之间分配工作负荷。 |
| 操作：并发传出调用数 | ~2,500 | 你可减少并发请求数，或根据需要减少持续时间。 |
| 运行时终结点：并发传入调用数 | ~1,000 | 你可减少并发请求数，或根据需要减少持续时间。 |
| 运行时终结点：每 5 分钟读取调用  | 60,000 | 可根据需要在多个应用中分发工作负荷。 |
| 运行时终结点：每 5 分钟调用调用 | 45,000 | 可根据需要在多个应用中分发工作负荷。 |
| 每 5 分钟的内容吞吐量 | 600 MB | 可根据需要在多个应用中分发工作负荷。 |
||||

<!--Not Available on ### Integration service environment (ISE)-->

<a name="gateway-limits"></a>

## <a name="gateway-limits"></a>网关限制

Azure 逻辑应用支持通过网关执行写入操作（包括插入和更新）。 但是，这些操作存在[有效负载大小限制](https://docs.microsoft.com/data-integration/gateway/service-gateway-onprem#considerations)。

<a name="request-limits"></a>

## <a name="http-limits"></a>HTTP 限制

下面是单个传出或传入 HTTP 调用的限制：

#### <a name="timeout"></a>超时

某些连接器操作会进行异步调用或侦听 Webhook 请求，因此，这些操作的超时时间可能会长于以下限制。 有关详细信息，请参阅特定连接器的技术详细信息以及[工作流触发器和操作](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action)。

<!--Not Available on Integration service environment limit-->

| 名称 | 多租户限制 | 注释 |
|------|--------------------|-------|
| 出站请求 | 120 秒 <br />（2 分钟） | 出站请求的示例包括 HTTP 触发器进行的调用。 <p><p>**提示**：对于运行时间较长的操作，请使用[异步轮询模式](../logic-apps/logic-apps-create-api-app.md#async-pattern)或 [until 循环](../logic-apps/logic-apps-workflow-actions-triggers.md#until-action)。 |
| 入站请求 | 120 秒 <br />（2 分钟） | 入站请求的示例包括请求触发器和 webhook 触发器收到的请求。 <p><p>**注意**：要使原始调用方能够获得响应，则除非以嵌套工作流的形式调用其他逻辑应用，否则必须在限制内完成响应的所有步骤。 有关详细信息，请参阅[调用、触发器或嵌套逻辑应用](../logic-apps/logic-apps-http-endpoint.md)。 |
|||||

<a name="message-size-limits"></a>

#### <a name="message-size"></a>消息大小

<!--Not Available on ISE-labeled connectors use the ISE limit, not their non-ISE connector limits.-->

| 名称 | 多租户限制 | 集成服务环境限制 | 注释 |
|------|--------------------|---------------------------------------|-------|
| 消息大小 | 100 MB | 200 MB | 若要解决此限制问题，请参阅[使用分块处理大型消息](../logic-apps/logic-apps-handle-large-messages.md)。 但是，某些连接器和 API 可能不支持分块，甚至不支持默认限制。 |
| 使用分块的消息大小 | 1 GB | 5 GB | 此限制适用于本机支持分块或可以在其运行时配置中启用分块的操作。 <p><p>对于集成服务环境，逻辑应用引擎支持此限制，但连接器有其自己的分块限制（不得超出引擎限制）。如需示例，请参阅 [Azure Blob 存储连接器的 API 参考](https://docs.microsoft.com/connectors/azureblob/)。 有关分块的详细信息，请参阅[使用分块处理大型消息](../logic-apps/logic-apps-handle-large-messages.md)。 |
|||||   

#### <a name="character-limits"></a>字符限制

| 名称 | 注释 |
|------|-------|
| 表达式计算限制 | 131,072 个字符 | `@concat()`、`@base64()`、`@string()` 表达式的长度不能超过此限制。 |
| 请求 URL 字符限制 | 16,384 个字符 |
|||

<a name="retry-policy-limits"></a>
#### <a name="retry-policy"></a>重试策略

| 名称 | 限制 | 注释 |
| ---- | ----- | ----- |
| 重试次数 | 90 | 默认值为 4。 若要更改默认值，请使用[重试策略参数](../logic-apps/logic-apps-workflow-actions-triggers.md)。 |
| 重试最大延迟 | 1 天 | 若要更改默认值，请使用[重试策略参数](../logic-apps/logic-apps-workflow-actions-triggers.md)。 |
| 重试最小延迟 | 5 秒 | 若要更改默认值，请使用[重试策略参数](../logic-apps/logic-apps-workflow-actions-triggers.md)。 |
||||

<a name="custom-connector-limits"></a>

## <a name="custom-connector-limits"></a>自定义连接器限制

下面介绍对可通过 Web API 创建的自定义连接器的限制。

<!--Not Available on Integration service environment limit-->

| 名称 | 多租户限制 | 注释 |
|------|--------------------|-------|
| 自定义连接器数 | 每个 Azure 订阅 1,000 | |
| 自定义连接器的每分钟请求数 | 每分钟每个连接 500 个请求 | 每分钟每个自定义连接器 2,000 个请求  |
|||

<a name="managed-identity"></a>

## <a name="managed-identities"></a>托管标识

<!--Not Available on user-assigned identity-->

| 名称 | 限制 |
|------|-------|
| 每个逻辑应用的托管标识 | 系统分配的标识 |
| 每个区域的每个 Azure 订阅中具有托管标识的逻辑应用数量 | 250 |
|||

<a name="integration-account-limits"></a>

<!--Pending Upgrade on Production Team ## Integration account limits-->

<a name="artifact-number-limits"></a>

### <a name="artifact-limits-per-integration-account"></a>每个集成帐户的项目限制

下面介绍对每个集成帐户层的项目数量限制。
有关定价费率，请参阅[逻辑应用定价](https://www.azure.cn/pricing/details/logic-apps/)。 若要了解集成帐户的定价和计费工作原理，请参阅[逻辑应用定价模型](../logic-apps/logic-apps-pricing.md#integration-accounts)。

> [!NOTE]
> 仅将免费层用于探索应用场景，而非生产应用场景。 此层限制吞吐量和使用情况，并且不具有服务级别协议 (SLA)。

| 项目 | 免费 | 基本 | 标准 |
|----------|------|-------|----------|
| EDI 贸易协议 | 10 个 | 1 | 1,000 |
| EDI 参与方 | 25 | 2 | 1,000 |
| 地图 | 25 | 500 | 1,000 |
| 架构 | 25 | 500 | 1,000 |
| 程序集 | 10 个 | 25 | 1,000 |
| 证书 | 25 | 2 | 1,000 |
| 批处理配置 | 5 | 1 | 50 |
||||

<a name="artifact-capacity-limits"></a>

### <a name="artifact-capacity-limits"></a>项目容量限制

| 项目 | 限制 | 注释 |
| -------- | ----- | ----- |
| Assembly | 8 MB | 若要上传大于 2 MB 的文件，请使用 [Azure 存储帐户和 blob 容器](../logic-apps/logic-apps-enterprise-integration-schemas.md)。 |
| 映射（XSLT 文件） | 8 MB | 若要上传大于 2 MB 的文件，请使用 [Azure 逻辑应用 REST API - 映射](https://docs.microsoft.com/rest/api/logic/maps/createorupdate)。 <p><p>**注意**：映射可以成功处理的数据或记录量取决于 Azure 逻辑应用中的消息大小和操作超时限制。 例如，如果使用 HTTP 操作，则根据 [HTTP 消息大小和超时限制](#request-limits)，在操作能够在 HTTP 超时限制内完成的情况下，映射最多可以处理达到 HTTP 消息大小限制的数据量。 |
| 架构 | 8 MB | 若要上传大于 2 MB 的文件，请使用 [Azure 存储帐户和 blob 容器](../logic-apps/logic-apps-enterprise-integration-schemas.md)。 |
||||

<a name="integration-account-throughput-limits"></a>

### <a name="throughput-limits"></a>吞吐量限制

<!--Not Available on FREE INTEGRATION ACCOUNT-->

| 运行时终结点 | 基本 | 标准 | 注释 |
|------------------|-------|----------|-------|
| 每 5 分钟读取调用 | 30,000 | 60,000 | 你可根据需要在多个帐户之间分配工作负荷。 |
| 每 5 分钟调用调用 | 30,000 | 45,000 | 你可根据需要在多个帐户之间分配工作负荷。 |
| 每 5 分钟跟踪调用 | 30,000 | 45,000 | 你可根据需要在多个帐户之间分配工作负荷。 |
| 阻止并发调用 | ~1,000 | ~1,000 | 对于所有 SKU 均相同。 你可减少并发请求数，或根据需要减少持续时间。 |
||||

<a name="b2b-protocol-limits"></a>

### <a name="b2b-protocol-as2-x12-edifact-message-size"></a>B2B 协议（AS2、X12、EDIFACT）消息大小

以下消息大小限制适用于 B2B 协议：

<!--Not Available on Integration service environment limit-->

| 名称 | 多租户限制 | 注释 |
|------|--------------------|-------|
| AS2 | v2 - 100 MB<br />v1 - 50 MB | 适用于解码和编码 |
| X12 | 50 MB | 适用于解码和编码 |
| EDIFACT | 50 MB | 适用于解码和编码 |
||||

<a name="disable-delete"></a>

## <a name="disabling-or-deleting-logic-apps"></a>禁用或删除逻辑应用

禁用逻辑应用后，任何新运行都不会实例化。
所有正在进行的和挂起的运行将继续进行，直到完成，这可能要花费一些时间才能完成。

删除逻辑应用后，任何新运行都不会实例化。
所有正在进行和挂起的运行都将取消。
如果有成千上万个运行，取消操作可能需要很长时间才能完成。

<a name="configuration"></a>

## <a name="firewall-configuration-ip-addresses-and-service-tags"></a>防火墙配置：IP 地址和服务标记

Azure 逻辑应用用于传入和传出调用的 IP 地址取决于逻辑应用所在的区域。  同一区域中的所有逻辑应用都使用相同的 IP 地址范围。

> [!TIP]
> 为了帮助降低创建安全规则时的复杂性，你可以选择使用[服务标记](../virtual-network/service-tags-overview.md)，而不是为每个区域指定逻辑应用 IP 地址，本部分稍后将对此进行介绍。 这些标记适用于可使用逻辑应用服务的区域：
>
> * **LogicAppsManagement**：表示逻辑应用服务的入站 IP 地址前缀。
> * **LogicApps**：表示逻辑应用服务的出站 IP 地址前缀。

<a name="inbound"></a>
<a name="outbound"></a>

> [!NOTE]
> 可以在 [Azure IP 范围和服务标记 - 中国云](https://www.microsoft.com/download/confirmation.aspx?id=57062)中按服务标记找到入站和出站 IP 地址。
>

* 若要支持逻辑应用通过 [HTTP](../connectors/connectors-native-http.md)、[HTTP + Swagger](../connectors/connectors-native-http-swagger.md) 和其他 HTTP 请求直接发出的调用，请根据这些逻辑应用所在的区域，使用逻辑应用服务所用的所有[入站](#inbound)和  [出站](#outbound) IP 地址对防火墙进行设置。 这些地址显示在本部分的“入站”  和“出站”  标题下，并按区域进行排序。

* 若要支持 [Azure 托管的连接器](../connectors/apis-list.md)发出的调用，请根据逻辑应用所在的区域，使用这些连接器所用的所有  [出站](#outbound) IP 地址对防火墙进行设置。 这些地址显示在本部分的“出站”  标题下，并按区域进行排序。

    <!--Not Available on [open these ports](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#network-ports-for-ise)-->

* 如果逻辑应用无法访问使用[防火墙和防火墙规则](../storage/common/storage-network-security.md)的 Azure 存储帐户，则可通过[各种选项来启用访问权限](../connectors/connectors-create-api-azureblobstorage.md#access-storage-accounts-behind-firewalls)。

    例如，逻辑应用不能直接访问使用防火墙规则的存储帐户，因此存在于同一区域中。 但是，如果允许[区域中托管连接器的出站 IP 地址](../logic-apps/logic-apps-limits-and-config.md#outbound)，则逻辑应用可以访问其他区域中的存储帐户，除非你使用 Azure 表存储或 Azure 队列存储连接器。 若要访问表存储或队列存储，则可改用 HTTP 触发器和操作。 有关其他选项，请参阅[访问防火墙后的存储帐户](../connectors/connectors-create-api-azureblobstorage.md#access-storage-accounts-behind-firewalls)。

* 对于自定义连接器和 [Azure 中国世纪互联](https://docs.microsoft.com/azure/china/)，固定或保留 IP 地址不可用。





## <a name="next-steps"></a>后续步骤  

* 了解如何[创建第一个逻辑应用](../logic-apps/quickstart-create-first-logic-app-workflow.md)  
* 了解[常见示例和方案](../logic-apps/logic-apps-examples-and-scenarios.md)

<!-- Update_Description: update meta properties, wording update, update link -->