---
title: 快速入门 - 创建具有多个公共 IP 地址的 Azure 防火墙 - 资源管理器模板
description: 了解如何使用资源管理器模板创建具有多个公共 IP 地址的 Azure 防火墙。
services: firewall
author: rockboyfor
ms.service: firewall
ms.topic: quickstart
ms.custom: subject-armqs
origin.date: 04/14/2020
ms.date: 08/03/2020
ms.testscope: yes|no
ms.testdate: 08/03/2020Null
ms.author: v-yeche
ms.openlocfilehash: 5ebfc8b3d9dbbcd4afde1d7f62ef50fdd7b3952e
ms.sourcegitcommit: 362814dc7ac5b56cf0237b9016a67c35d8d72c32
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2020
ms.locfileid: "87455589"
---
<!--Verified successfully on 05/25/2020-->
<!--Successfully on Mooncake portal-->
# <a name="quickstart-create-an-azure-firewall-with-multiple-public-ip-addresses---resource-manager-template"></a>快速入门：创建具有多个公共 IP 地址的 Azure 防火墙 - 资源管理器模板

在本快速入门中，我们使用资源管理器模板部署具有多个公共 IP 地址的 Azure 防火墙。 部署的防火墙具有 NAT 规则收集规则，这些规则允许通过 RDP 连接与两个 Windows Server 2019 虚拟机进行连接。

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

若要详细了解具有多个公共 IP 地址的 Azure 防火墙，请参阅[使用 Azure PowerShell 部署具有多个公共 IP 地址的 Azure 防火墙](deploy-multi-public-ip-powershell.md)。

## <a name="prerequisites"></a>先决条件

- 具有活动订阅的 Azure 帐户。 [免费创建帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="create-an-azure-firewall"></a>创建 Azure 防火墙

此模板创建具有两个公共 IP 地址的 Azure 防火墙，以及用于支持 Azure 防火墙的必要资源。

### <a name="review-the-template"></a>查看模板

本快速入门中使用的模板来自 [Azure 快速入门模板](https://github.com/Azure/azure-quickstart-templates/tree/master/fw-docs-qs)。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminUsername": {
            "type": "String",
            "metadata": {
                "description": "Admin username for the backend servers"
            }
        },
        "adminPassword": {
            "type": "SecureString",
            "metadata": {
                "description": "Password for the admin account on the backend servers"
            }
        },
        "location": {
            "defaultValue": "[resourceGroup().location]",
            "type": "String",
            "metadata": {
                "description": "Location for all resources."
            }
        },
        "vmSize": {
            "defaultValue": "Standard_B2ms",
            "type": "String",
            "metadata": {
                "description": "Size of the virtual machine."
            }
        }
    },
    "variables": {
        "virtualMachines_myVM_name": "myVM",
        "virtualNetworks_myVNet_name": "myVNet",
        "net_interface": "net-int",
        "ipconfig_name": "ipconfig",
        "publicIPAddress": "public_ip",
        "nsg_name": "vm-nsg",
        "firewall_name": "FW-01",
        "vnet_prefix": "10.0.0.0/16",
        "fw_subnet_prefix": "10.0.0.0/24",
        "backend_subnet_prefix": "10.0.1.0/24",
        "azureFirewallSubnetId": "[resourceId('Microsoft.Network/virtualNetworks/subnets',variables('virtualNetworks_myVNet_name'), 'AzureFirewallSubnet')]",
        "azureFirewallSubnetJSON": "[json(format('{{\"id\": \"{0}\"}}', variables('azureFirewallSubnetId')))]",
        "copy": [
            {
                "name": "azureFirewallIpConfigurations",
                "count": 2,
                "input": {
                    "name": "[concat('IpConf', copyIndex('azureFirewallIpConfigurations',1))]",
                    "properties": {
                        "subnet": "[if(equals(copyIndex('azureFirewallIpConfigurations',1), 1), variables('azureFirewallSubnetJSON'), json('null'))]",
                        "publicIPAddress": {
                            "id": "[resourceId('Microsoft.Network/publicIPAddresses', concat(variables('publicIPAddress'), copyIndex('azureFirewallIpConfigurations',1)))]"
                        }
                    }
                }
            }
        ]
    },
    "resources": [
        {
            "type": "Microsoft.Network/networkSecurityGroups",
            "apiVersion": "2019-11-01",
            "name": "[concat(variables('nsg_name'), copyIndex(1))]",
            "location": "[parameters('location')]",
            "copy": {
                "name": "nsg-loop",
                "count": 2
            },
            "properties": {
                "securityRules": [
                    {
                        "name": "RDP",
                        "properties": {
                            "protocol": "TCP",
                            "sourcePortRange": "*",
                            "destinationPortRange": "3389",
                            "sourceAddressPrefix": "*",
                            "destinationAddressPrefix": "*",
                            "access": "Allow",
                            "priority": 300,
                            "direction": "Inbound"
                        }
                    }
                ]
            }

        },
        {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2019-11-01",
            "name": "[concat(variables('publicIPAddress'), copyIndex(1))]",
            "location": "[parameters('location')]",
            "sku": {
                "name": "Standard"
            },
            "copy": {
                "name": "publicip-loop",
                "count": 2
            },
            "properties": {
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Static",
                "idleTimeoutInMinutes": 4
            }

        },
        {
            "type": "Microsoft.Network/virtualNetworks",
            "apiVersion": "2019-11-01",
            "name": "[variables('virtualNetworks_myVNet_name')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/routeTables', 'rt-01')]"
            ],
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "[variables('vnet_prefix')]"
                    ]
                },
                "subnets": [
                    {
                        "name": "myBackendSubnet",
                        "properties": {
                            "addressPrefix": "[variables('backend_subnet_prefix')]",
                            "routeTable": {
                                "id": "[resourceId('Microsoft.Network/routeTables', 'rt-01')]"
                            },
                            "privateEndpointNetworkPolicies": "Enabled",
                            "privateLinkServiceNetworkPolicies": "Enabled"
                        }
                    }
                ],
                "enableDdosProtection": false,
                "enableVmProtection": false
            }
        },
        {
            "type": "Microsoft.Network/virtualNetworks/subnets",
            "apiVersion": "2019-11-01",
            "name": "[concat(variables('virtualNetworks_myVNet_name'), '/AzureFirewallSubnet')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/virtualNetworks', variables('virtualNetworks_myVNet_name'))]"
            ],
            "properties": {
                "addressPrefix": "[variables('fw_subnet_prefix')]",
                "privateEndpointNetworkPolicies": "Enabled",
                "privateLinkServiceNetworkPolicies": "Enabled"
            }
        },
        {
            "type": "Microsoft.Compute/virtualMachines",
            "apiVersion": "2019-07-01",
            "name": "[concat(variables('virtualMachines_myVM_name'), copyIndex(1))]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('net_interface'), copyIndex(1)))]"
            ],
            "copy": {
                "name": "vm-loop",
                "count": 2
            },
            "properties": {
                "hardwareProfile": {
                    "vmSize": "[parameters('vmSize')]"
                },
                "storageProfile": {
                    "imageReference": {
                        "publisher": "MicrosoftWindowsServer",
                        "offer": "WindowsServer",
                        "sku": "2019-Datacenter",
                        "version": "latest"
                    },
                    "osDisk": {
                        "osType": "Windows",
                        "createOption": "FromImage",
                        "caching": "ReadWrite",
                        "managedDisk": {
                            "storageAccountType": "StandardSSD_LRS"
                        },
                        "diskSizeGB": 127
                    }
                },
                "osProfile": {
                    "computerName": "[concat(variables('virtualMachines_myVM_name'), copyIndex(1))]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]",
                    "windowsConfiguration": {
                        "provisionVMAgent": true,
                        "enableAutomaticUpdates": true
                    },
                    "allowExtensionOperations": true
                },
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('net_interface'), copyIndex(1)))]"
                        }
                    ]
                }
            }

        },
        {
            "type": "Microsoft.Network/networkInterfaces",
            "apiVersion": "2019-11-01",
            "name": "[concat(variables('net_interface'), copyIndex(1))]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/virtualNetworks', variables('virtualNetworks_myVNet_name'))]",
                "[resourceId('Microsoft.Network/networkSecurityGroups', concat(variables('nsg_name'), copyIndex(1)))]"
            ],
            "copy": {
                "name": "int-loop",
                "count": 2
            },
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "[concat(variables('ipconfig_name'), copyIndex(1))]",
                        "properties": {
                            "subnet": {
                                "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', variables('virtualNetworks_myVNet_name'), 'myBackendSubnet')]"
                            },
                            "primary": true
                        }
                    }
                ],
                "enableAcceleratedNetworking": false,
                "enableIPForwarding": false,
                "networkSecurityGroup": {
                    "id": "[resourceId('Microsoft.Network/networkSecurityGroups', concat(variables('nsg_name'), copyIndex(1)))]"
                }
            }
        },
        {
            "type": "Microsoft.Network/azureFirewalls",
            "apiVersion": "2019-11-01",
            "name": "[variables('firewall_name')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/publicIPAddresses', concat(variables('publicIPAddress'), 1))]",
                "[resourceId('Microsoft.Network/publicIPAddresses', concat(variables('publicIPAddress'), 2))]",
                "[resourceId('Microsoft.Network/virtualNetworks/subnets', variables('virtualNetworks_myVNet_name'), 'AzureFirewallSubnet')]"

            ],
            "properties": {
                "sku": {
                    "name": "AZFW_VNet",
                    "tier": "Standard"
                },
                "threatIntelMode": "Alert",
                "ipConfigurations": "[variables('azureFirewallIpConfigurations')]",
                "applicationRuleCollections": [
                    {
                        "name": "web",
                        "properties": {
                            "priority": 100,
                            "action": {
                                "type": "Allow"
                            },
                            "rules": [
                                {
                                    "name": "wan-address",
                                    "protocols": [
                                        {
                                            "protocolType": "Http",
                                            "port": 80
                                        },
                                        {
                                            "protocolType": "Https",
                                            "port": 443
                                        }
                                    ],
                                    "targetFqdns": [
                                        "getmywanip.com"
                                    ],
                                    "sourceAddresses": [
                                        "*"
                                    ]
                                },
                                {
                                    "name": "google",
                                    "protocols": [
                                        {
                                            "protocolType": "Http",
                                            "port": 80
                                        },
                                        {
                                            "protocolType": "Https",
                                            "port": 443
                                        }
                                    ],
                                    "targetFqdns": [
                                        "www.google.com"
                                    ],
                                    "sourceAddresses": [
                                        "10.0.1.0/24"
                                    ]
                                },
                                {
                                    "name": "wupdate",
                                    "protocols": [
                                        {
                                            "protocolType": "Http",
                                            "port": 80
                                        },
                                        {
                                            "protocolType": "Https",
                                            "port": 443
                                        }
                                    ],
                                    "fqdnTags": [
                                        "WindowsUpdate"
                                    ],
                                    "sourceAddresses": [
                                        "*"
                                    ]
                                }
                            ]
                        }
                    }
                ],
                "natRuleCollections": [
                    {
                        "name": "Coll-01",
                        "properties": {
                            "priority": 100,
                            "action": {
                                "type": "Dnat"
                            },
                            "rules": [
                                {
                                    "name": "rdp-01",
                                    "protocols": [
                                        "TCP"
                                    ],
                                    "translatedAddress": "10.0.1.4",
                                    "translatedPort": "3389",
                                    "sourceAddresses": [
                                        "*"
                                    ],
                                    "destinationAddresses": [ "[reference(resourceId('Microsoft.Network/publicIPAddresses/', concat(variables('publicIPAddress'), 1))).ipAddress]" ],
                                    "destinationPorts": [
                                        "3389"
                                    ]
                                },
                                {
                                    "name": "rdp-02",
                                    "protocols": [
                                        "TCP"
                                    ],
                                    "translatedAddress": "10.0.1.5",
                                    "translatedPort": "3389",
                                    "sourceAddresses": [
                                        "*"
                                    ],
                                    "destinationAddresses": [ "[reference(resourceId('Microsoft.Network/publicIPAddresses/', concat(variables('publicIPAddress'), 2))).ipAddress]" ],
                                    "destinationPorts": [
                                        "3389"
                                    ]
                                }
                            ]
                        }
                    }
                ]
            }
        },
        {
            "type": "Microsoft.Network/routeTables",
            "apiVersion": "2019-11-01",
            "name": "rt-01",
            "location": "[parameters('location')]",
            "properties": {
                "disableBgpRoutePropagation": false,
                "routes": [
                    {
                        "name": "fw",
                        "properties": {
                            "addressPrefix": "0.0.0.0/0",
                            "nextHopType": "VirtualAppliance",
                            "nextHopIpAddress": "10.0.0.4"
                        }
                    }
                ]
            }
        }

    ]
```

模板中定义了多个 Azure 资源：

- **Microsoft.Network/publicIPAddresses**
- **Microsoft.Network/networkSecurityGroups**
- **Microsoft.Network/virtualNetworks**
- Microsoft.Compute/virtualMachines 
- **Microsoft.Network/networkInterfaces**
- **Microsoft.Storage/storageAccounts**
- **Microsoft.Network/azureFirewalls**
- **Microsoft.Network/routeTables**

<!--Not Available on Microsoft Template Link->

### Deploy the template

Deploy Resource Manager template to Azure:

1. Select **Deploy to Azure** to sign in to Azure and open the template. The template creates an Azure Firewall, the network infrastructure, and two virtual machines.

    [![Deploy to Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.cn/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Ffw-docs-qs%2Fazuredeploy.json)

2. In the portal, on the **Create an Azure Firewall with multiple IP public addresses** page, type or select the following values:
    - Subscription: Select from existing subscriptions 
    - Resource group:  Select from existing resource groups or select **Create new**, and select **OK**.
    - Location: Select a location
    - Admin Username: Type username for the administrator user account 
    - Admin Password: Type an administrator password or key

3. Select **I agree to the terms and conditions stated above** and then select **Purchase**. The deployment can take 10 minutes or longer to complete.

## Validate the deployment

In the Azure portal, review the deployed resources. Note the firewall public IP addresses.  

Use Remote Desktop Connection to connect to the firewall public IP addresses. Successful connections demonstrates firewall NAT rules that allow the connection to the backend servers.

## Clean up resources

When you no longer need the resources that you created with the firewall, delete the resource group. This removes the firewall and all the related resources.

To delete the resource group, call the `Remove-AzResourceGroup` cmdlet:

```powershell
Remove-AzResourceGroup -Name "<your resource group name>"
```

## Next steps

> [!div class="nextstepaction"]
> [Tutorial: Deploy and configure Azure Firewall in a hybrid network using the Azure portal](tutorial-hybrid-portal.md)

<!-- Update_Description: update meta properties, wording update, update link -->