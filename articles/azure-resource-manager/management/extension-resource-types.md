---
title: 扩展资源类型
description: 列出用于扩展其他资源类型的功能的 Azure 资源类型。
ms.topic: conceptual
origin.date: 04/06/2020
ms.date: 04/30/2020
ms.author: v-yeche
ms.openlocfilehash: 9c5a8de30143f21ee0c117393fc0617d19f367a9
ms.sourcegitcommit: b469d275694fb86bbe37a21227e24019043b9e88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82596091"
---
<!--Verified successfully/Redirect file-->
# <a name="resource-types-that-extend-capabilities-of-other-resources"></a>用于扩展其他资源的功能的资源类型

扩展资源是扩展另一资源的功能的资源。 例如，资源锁是扩展资源。 对另一资源应用资源锁可以防止该资源被删除或修改。 单独创建资源锁没有意义。 扩展资源始终会应用到其他资源。

## <a name="extension-resource-types"></a>扩展资源类型

- Microsoft.Advisor/configurations
- Microsoft.Advisor/recommendations
- Microsoft.Advisor/suppressions
- Microsoft.AlertsManagement/alerts
- Microsoft.AlertsManagement/alertsSummary
- Microsoft.Authorization/checkAccess
- Microsoft.Authorization/denyAssignments
- Microsoft.Authorization/findOrphanRoleAssignments
- Microsoft.Authorization/locks
- Microsoft.Authorization/permissions
- Microsoft.Authorization/policyAssignments
- Microsoft.Authorization/policyDefinitions
- Microsoft.Authorization/policySetDefinitions
- Microsoft.Authorization/roleAssignments
- Microsoft.Authorization/roleAssignmentsUsageMetrics
- Microsoft.Authorization/roleDefinitions

    <!--Not Available on - Microsoft.Billing/*-->
    <!--Not Available on - Microsoft.Blueprint/*-->
    <!--Not Available on - Microsoft.Consumption/*-->

- Microsoft.ContainerInstance/serviceAssociationLinks

    <!--Not Available on - Microsoft.CostManagement/*-->
    <!--Not Available on - Microsoft.CustomProviders/*-->
    
- Microsoft.EventGrid/eventSubscriptions
- Microsoft.EventGrid/extensionTopics
- Microsoft.GuestConfiguration/configurationProfileAssignments
- Microsoft.GuestConfiguration/guestConfigurationAssignments
- Microsoft.GuestConfiguration/software
- Microsoft.GuestConfiguration/softwareUpdateProfile
- Microsoft.GuestConfiguration/softwareUpdates
- microsoft.insights/baseline
- microsoft.insights/calculatebaseline
- microsoft.insights/dataCollectionRuleAssociations
- microsoft.insights/diagnosticSettings
- microsoft.insights/diagnosticSettingsCategories
- microsoft.insights/eventtypes
- microsoft.insights/extendedDiagnosticSettings
- microsoft.insights/guestDiagnosticSettingsAssociation
- microsoft.insights/logDefinitions
- microsoft.insights/logs
- microsoft.insights/metricDefinitions
- microsoft.insights/metricNamespaces
- microsoft.insights/metricbaselines
- microsoft.insights/metrics
- microsoft.insights/myWorkbooks
- microsoft.insights/topology
- microsoft.insights/transactions
- microsoft.insights/vmInsightsOnboardingStatuses
    
    <!--Not Available on - Microsoft.KubernetesConfiguration/*-->
    
- Microsoft.Maintenance/applyUpdates
- Microsoft.Maintenance/configurationAssignments
- Microsoft.Maintenance/updates
- Microsoft.ManagedIdentity/Identities

    <!--Not Available on - Microsoft.ManagedServices/*-->
    
- Microsoft.OperationalInsights/storageInsightConfigs
- Microsoft.OperationsManagement/managementassociations
- Microsoft.PolicyInsights/policyEvents
- Microsoft.PolicyInsights/policyStates
- Microsoft.PolicyInsights/policyTrackedResources
- Microsoft.PolicyInsights/remediations
- Microsoft.RecoveryServices/backupProtectedItems
- Microsoft.RecoveryServices/replicationEligibilityResults
- Microsoft.ResourceHealth/availabilityStatuses
- Microsoft.ResourceHealth/childAvailabilityStatuses
- Microsoft.ResourceHealth/childResources
- Microsoft.ResourceHealth/events
- Microsoft.ResourceHealth/impactedResources
- Microsoft.ResourceHealth/notifications
- Microsoft.Resources/links
- Microsoft.Resources/tags
- Microsoft.Security/Compliances
- Microsoft.Security/InformationProtectionPolicies
- Microsoft.Security/adaptiveNetworkHardenings
- Microsoft.Security/advancedThreatProtectionSettings
- Microsoft.Security/assessmentMetadata
- Microsoft.Security/assessments
- Microsoft.Security/complianceResults
- Microsoft.Security/dataCollectionAgents
- Microsoft.Security/deviceSecurityGroups
- Microsoft.Security/networkData
- Microsoft.Security/serverVulnerabilityAssessments

    <!--Not Available on - Microsoft.SecurityInsights/*-->
    <!--Not Available on - Microsoft.SoftwarePlan/*-->
    <!--Not Available on - Microsoft.Subscription/*-->
    <!--Not Available on - microsoft.support/*-->
    <!--Not Available on - Microsoft.WorkloadMonitor/*-->

## <a name="next-steps"></a>后续步骤

- 若要在 Azure 资源管理器模板中获取扩展资源的资源 ID，请使用 [extensionResourceId](../templates/template-functions-resource.md#extensionresourceid)。

<!--Not Available on [Event Grid Event Subscriptions](https://docs.microsoft.com/azure/templates/microsoft.eventgrid/2019-06-01/eventsubscriptions)-->

<!-- Update_Description: new article about extension resource types -->
<!--NEW.date: 03/09/2020-->