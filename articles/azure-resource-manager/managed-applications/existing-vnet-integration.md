---
title: 部署到现有虚拟网络
description: 介绍如何允许托管应用程序的用户选择现有虚拟网络。 虚拟网络可以在托管应用程序之外。
author: rockboyfor
ms.topic: conceptual
origin.date: 05/11/2020
ms.date: 06/22/2020
ms.author: v-yeche
ms.openlocfilehash: 08d04362899960aee928f17cff47fc3aa85e8650
ms.sourcegitcommit: 48b5ae0164f278f2fff626ee60db86802837b0b4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85102080"
---
<!--Verified successfully on 2020/06/18 by harris-->
# <a name="use-existing-virtual-network-with-azure-managed-applications"></a>通过 Azure 托管应用程序使用现有虚拟网络

本文介绍如何定义与使用者的订阅中的现有虚拟网络集成的 Azure 托管应用程序。 托管应用程序让使用者决定是创建新的虚拟网络还是使用现有的虚拟网络。 现有虚拟网络可以位于托管资源组之外。

## <a name="main-template"></a>主模板

首先，让我们看一下 mainTemplate.json 文件。 下面显示了用于部署虚拟机及其关联资源的整个模板。 稍后，你将更仔细地检查模板中与使用现有虚拟网络相关的部分。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "type": "string",
      "metadata": {
        "description": "Deployment location"
      }
    },
    "windowsOSVersion": {
      "type": "string",
      "defaultValue": "2016-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter",
        "2016-Nano-Server",
        "2016-Datacenter-with-Containers",
        "2016-Datacenter",
        "2019-Datacenter"
      ],
      "metadata": {
        "description": "The Windows version for the VM. This will pick a fully patched image of this given Windows version."
      }
    },
    "vmName": {
      "type": "string",
      "metadata": {
        "title": "VM Name",
        "description": "This is the name of the your VM"
      }
    },
    "adminUsername": {
      "type": "string",
      "defaultValue": "testadmin",
      "metadata": {
        "description": "Username for the Virtual Machine."
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for the Virtual Machine."
      }
    },
    "virtualNetworkName": {
      "type": "string",
      "metadata": {
        "description": "New or Existing VNet Name"
      }
    },
    "virtualNetworkNewOrExisting": {
      "type": "string",
      "metadata": {
        "description": "Boolean indicating whether the VNet is new or existing"
      }
    },
    "virtualNetworkAddressPrefix": {
      "type": "string",
      "metadata": {
        "description": "VNet address prefix"
      }
    },
    "virtualNetworkResourceGroup": {
      "type": "string",
      "metadata": {
        "description": "Resource group of the VNet"
      }
    },
    "virtualMachineSize": {
      "type": "string",
      "metadata": {
        "description": "The size of the VM"
      }
    },
    "subnetName": {
      "type": "string",
      "metadata": {
        "description": "New or Existing subnet Name"
      }
    },
    "subnetAddressPrefix": {
      "type": "string",
      "metadata": {
        "description": "Subnet address prefix"
      }
    },
    "baseUrl": {
      "type": "string",
      "defaultValue": "",
      "metadata": {
        "artifactsBaseUrl": "",
        "description": "URL to acquire other templates"
      }
    }
  },
  "variables": {
    

    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'sawinvm')]",
    "publicIPAddressName": "[concat(uniqueString(resourceGroup().id),'IP')]",
    "vmName": "[parameters('vmName')]",
    "nicName": "[concat(parameters('vmName'),'Nic')]",
    "vnetId": {
      "new": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
      "existing": "[resourceId(parameters('virtualNetworkResourceGroup'),'Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]"
    },
    "subnetId": "[concat(variables('vnetId')[parameters('virtualNetworkNewOrExisting')],'/subnets/',parameters('subnetName'))]",
    "publicIPAddressType": "Dynamic"
  },
  "resources": [
    {
      "condition": "[equals(parameters('virtualNetworkNewOrExisting'),'new')]",
      "type": "Microsoft.Network/virtualNetworks",
      "apiVersion": "2017-09-01",
      "name": "[parameters('virtualNetworkName')]",
      "location": "[parameters('location')]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[parameters('virtualNetworkAddressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[parameters('subnetName')]",
            "properties": {
              "addressPrefix": "[parameters('subnetAddressPrefix')]"
            }
          }
        ]
      }
    },
    {
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "apiVersion": "2015-06-15",
      "location": "[parameters('location')]",
      "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]"
      }
    },
    {
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "apiVersion": "2016-03-30",
      "location": "[parameters('location')]",
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
              },
              "subnet": {
                "id": "[variables('subnetId')]"
              }
            }
          }
        ],
        "enableIPForwarding": true
      },
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[resourceId('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
      ]
    },
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2018-11-01",
      "name": "[variables('storageAccountName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {}
    },
    {
      "type": "Microsoft.Compute/virtualMachines",
      "apiVersion": "2018-10-01",
      "name": "[variables('vmName')]",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": {
          "vmSize": "[parameters('virtualMachineSize')]"
        },
        "osProfile": {
          "computerName": "[variables('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "[parameters('windowsOSVersion')]",
            "version": "latest"
          },
          "osDisk": {
            "createOption": "FromImage"
          },
          "dataDisks": [
            {
              "diskSizeGB": 1023,
              "lun": 0,
              "createOption": "Empty"
            }
          ]
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        },
        "diagnosticsProfile": {
          "bootDiagnostics": {
            "enabled": true,
            "storageUri": "[reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob]"
          }
        }
      }
    }
  ]
}
```

请注意，虚拟网络是[有条件部署的](../templates/conditional-resource-deployment.md)。 使用者传入一个参数值，该值指示是创建新的虚拟网络还是使用现有的虚拟网络。 如果使用者选择新的虚拟网络，则将部署资源。 否则，在部署过程中将跳过资源。

```json
{
  "condition": "[equals(parameters('virtualNetworkNewOrExisting'),'new')]",
  "type": "Microsoft.Network/virtualNetworks",
  "apiVersion": "2017-09-01",
  "name": "[parameters('virtualNetworkName')]",
  "location": "[parameters('location')]",
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "[parameters('virtualNetworkAddressPrefix')]"
      ]
    },
    "subnets": [
      {
        "name": "[parameters('subnetName')]",
        "properties": {
          "addressPrefix": "[parameters('subnetAddressPrefix')]"
        }
      }
    ]
  }
},
```

虚拟网络 ID 的变量具有两个属性。 一个属性在部署新虚拟网络时返回资源 ID。 另一个属性在使用现有虚拟网络时返回资源 ID。 现有虚拟网络的资源 ID 包含虚拟网络所在资源组的名称。

子网 ID 根据虚拟网络 ID 的值进行构造。 它使用的值与使用者的选择匹配。

```json
"variables": {
  "storageAccountEndPoint": "https://core.chinacloudapi.cn/",

  "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'sawinvm')]",
  "publicIPAddressName": "[concat(uniqueString(resourceGroup().id),'IP')]",
  "vmName": "[parameters('vmName')]",
  "nicName": "[concat(parameters('vmName'),'Nic')]",
  "vnetId": {
    "new": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
    "existing": "[resourceId(parameters('virtualNetworkResourceGroup'),'Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]"
  },
  "subnetId": "[concat(variables('vnetId')[parameters('virtualNetworkNewOrExisting')],'/subnets/',parameters('subnetName'))]",
  "publicIPAddressType": "Dynamic"
},
```

网络接口将设置为子网 ID 变量。

```json
{
  "type": "Microsoft.Network/networkInterfaces",
  "name": "[variables('nicName')]",
  "apiVersion": "2016-03-30",
  "location": "[parameters('location')]",
  "properties": {
    "ipConfigurations": [
      {
        "name": "ipconfig1",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "publicIPAddress": {
            "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
          },
          "subnet": {
            "id": "[variables('subnetId')]"
          }
        }
      }
    ],
    "enableIPForwarding": true
  },
```

## <a name="ui-definition"></a>UI 定义

现在，让我们看一下 createUiDefinition.json 文件。 整个文件如下所示：

```json
{
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
  "parameters": {
    "basics": [],
    "steps": [
      {
        "name": "deploymentDetails",
        "label": "Deployment Details",
        "subLabel": {
          "preValidation": "Required",
          "postValidation": "Done"
        },
        "bladeTitle": "Deployment Details",
        "elements": [
          {
            "name": "virtualMachine",
            "type": "Microsoft.Common.Section",
            "elements": [
              {
                "name": "vmName",
                "type": "Microsoft.Common.TextBox",
                "label": "VM Name",
                "toolTip": "Name of your virtual machine",
                "constraints": {
                  "required": true
                }
              },
              {
                "name": "vmPassword",
                "type": "Microsoft.Compute.CredentialsCombo",
                "label": {
                  "password": "Password",
                  "confirmPassword": "Confirm password"
                },
                "toolTip": {
                  "password": ""
                },
                "constraints": {
                  "required": true,
                  "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{12,}$",
                  "customValidationMessage": "The password must be alphanumeric, contain at least 12 characters, and have at least 1 letter and 1 number."
                },
                "options": {
                  "hideConfirmation": false
                },
                "osPlatform": "Windows",
                "visible": true
              }
            ],
            "visible": true
          },
          {
            "name": "vnet",
            "type": "Microsoft.Network.VirtualNetworkCombo",
            "label": {
              "virtualNetwork": "Virtual Network",
              "subnets": "Subnets"
            },
            "defaultValue": {
              "name": "vmx-vnet",
              "addressPrefixSize": "/16"
            },
            "constraints": {
              "minAddressPrefixSize": "/24"
            },
            "subnets": {
              "subnet1": {
                "label": "Subnet",
                "defaultValue": {
                  "name": "vmx-subnet",
                  "addressPrefixSize": "/24"
                },
                "constraints": {
                  "minAddressPrefixSize": "/29",
                  "minAddressCount": 8,
                  "requireContiguousAddresses": true
                }
              }
            }
          },
          {
            "name": "VMSize",
            "type": "Microsoft.Compute.SizeSelector",
            "label": "VM size",
            "toolTip": "The size of virtual machine for VM.",
            "recommendedSizes": [
              "Standard_D2_v2",
              "Standard_D2_v3"
            ],
            "constraints": {
              "allowedSizes": [
                "Standard_D2_v2",
                "Standard_D2_v3"
              ],
              "excludedSizes": []
            },
            "osPlatform": "Windows",
            "imageReference": {
              "publisher": "MicrosoftWindowsServer",
              "offer": "WindowsServer",
              "sku": "2012-R2-Datacenter"
            }
          }
        ]
      },
      {
        "name": "identityDetails",
        "label": "Managed Identity Details",
        "subLabel": {
          "preValidation": "Required",
          "postValidation": "Done"
        },
        "bladeTitle": "Managed Identity Details",
        "elements": [
          {
            "name": "identity",
            "type": "Microsoft.ManagedIdentity.IdentitySelector",
            "label": "Managed Identity Configuration",
            "toolTip": {
              "systemAssignedIdentity": "Enable system assigned identity to grant the resource access to other existing resources.",
              "userAssignedIdentity": "Add user assigned identities to grant the resource access to other existing resources."
            },
            "defaultValue": {
              "systemAssignedIdentity": "Off"
            },
            "options": {
              "hideSystemAssignedIdentity": false,
              "hideUserAssignedIdentity": false
            },
            "visible": true
          }
        ]
      }
    ],
    "outputs": {
      "location": "[location()]",
      "vmName": "[steps('deploymentDetails').virtualMachine.vmName]",
      "adminPassword": "[steps('deploymentDetails').virtualMachine.vmPassword.password]",
      "virtualNetworkName": "[steps('deploymentDetails').vnet.name]",
      "virtualNetworkNewOrExisting": "[steps('deploymentDetails').vnet.newOrExisting]",
      "virtualNetworkAddressPrefix": "[first(steps('deploymentDetails').vnet.addressPrefixes)]",
      "virtualNetworkResourceGroup": "[steps('deploymentDetails').vnet.resourceGroup]",
      "virtualMachineSize": "[steps('deploymentDetails').VMSize]",
      "subnetName": "[steps('deploymentDetails').vnet.subnets.subnet1.name]",
      "subnetAddressPrefix": "[steps('deploymentDetails').vnet.subnets.subnet1.addressPrefix]",
      "managedIdentity": "[steps('identityDetails').identity]"
    }
  }
}
```

此文件包含一个虚拟网络元素。

```json
{
  "name": "vnet",
  "type": "Microsoft.Network.VirtualNetworkCombo",
  "label": {
    "virtualNetwork": "Virtual Network",
    "subnets": "Subnets"
  },
  "defaultValue": {
    "name": "vmx-vnet",
    "addressPrefixSize": "/16"
  },
  "constraints": {
    "minAddressPrefixSize": "/24"
  },
  "subnets": {
    "subnet1": {
      "label": "Subnet",
      "defaultValue": {
        "name": "vmx-subnet",
        "addressPrefixSize": "/24"
      },
      "constraints": {
        "minAddressPrefixSize": "/29",
        "minAddressCount": 8,
        "requireContiguousAddresses": true
      }
    }
  }
},
```

该元素让使用者选择新虚拟网络或现有虚拟网络。

:::image type="content" source="./media/existing-vnet-integration/new-or-existing-vnet.png" alt-text="新虚拟网络或现有虚拟网络":::

在输出中包含一个值，该值指示使用者是选择了新虚拟网络还是现有虚拟网络。 还有一个托管标识值。

> [!NOTE]
> 托管标识的输出值必须命名为“managedIdentity”。

```json
"outputs": {
  "location": "[location()]",
  "vmName": "[steps('deploymentDetails').virtualMachine.vmName]",
  "adminPassword": "[steps('deploymentDetails').virtualMachine.vmPassword.password]",
  "virtualNetworkName": "[steps('deploymentDetails').vnet.name]",
  "virtualNetworkNewOrExisting": "[steps('deploymentDetails').vnet.newOrExisting]",
  "virtualNetworkAddressPrefix": "[first(steps('deploymentDetails').vnet.addressPrefixes)]",
  "virtualNetworkResourceGroup": "[steps('deploymentDetails').vnet.resourceGroup]",
  "virtualMachineSize": "[steps('deploymentDetails').VMSize]",
  "subnetName": "[steps('deploymentDetails').vnet.subnets.subnet1.name]",
  "subnetAddressPrefix": "[steps('deploymentDetails').vnet.subnets.subnet1.addressPrefix]",
  "managedIdentity": "[steps('identityDetails').identity]"
}
```

## <a name="next-steps"></a>后续步骤

若要详细了解如何创建 UI 定义文件，请参阅[适合 Azure 托管应用程序的创建体验的 CreateUiDefinition.json](create-uidefinition-overview.md)。

<!-- Update_Description: new article about existing vnet integration -->
<!--NEW.date: 06/22/2020-->