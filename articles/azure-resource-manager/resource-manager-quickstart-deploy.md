---
title: "将资源部署到 Azure | Microsoft Docs"
description: "使用 Azure PowerShell 或 Azure CLI 将资源部署到 Azure。 资源在 Resource Manager 模板中定义。"
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/16/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 3b3162c335be43c9bf2a3d1c14b86cdd9b5d46b9
ms.lasthandoff: 04/27/2017


---
# <a name="deploy-resources-to-azure"></a>将资源部署到 Azure

本主题演示如何将资源部署到 Azure 订阅。 可以使用 Azure PowerShell 或 Azure CLI 来部署用于定义解决方案基础结构的 Resource Manager 模板。

有关资源管理器概念的介绍，请参阅 [Azure Resource Manager 概述](resource-group-overview.md)。

## <a name="steps-for-deployment"></a>部署步骤

本主题假定用户将部署本主题中的[示例存储模板](#example-storage-template)。 可以使用其他模板，但传递的参数与本主题中的不同。

创建模板后，部署模板的常规步骤如下：

1. 登录到你的帐户
2. 选择要使用的订阅（仅当有多个订阅，并且要使用非默认订阅时需要执行此操作）
3. 创建资源组
4. 部署模板
5. 检查部署状态

以下部分说明如何使用 [PowerShell](#powershell) 或 [Azure CLI](#azure-cli) 来执行这些步骤。

## <a name="powershell"></a>PowerShell

1. 若要安装 Azure PowerShell，请参阅 [Azure PowerShell cmdlet 入门](/powershell/azure/overview)。

2. 若要快速开始进行部署，请使用以下 cmdlets：

  ```powershell
  Login-AzureRmAccount
  Set-AzureRmContext -SubscriptionID {subscription-id}

  New-AzureRmResourceGroup -Name ExampleGroup -Location "Central US"
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json 
  ```

  仅当要使用非默认订阅时，才需要 `Set-AzureRmContext` cmdlet。 若要查看所有订阅及其 ID，请使用：

  ```powershell
  Get-AzureRmSubscription
  ```

3. 部署可能需要几分钟才能完成。 完成之后，将看到如下消息：

  ```powershell
  DeploymentName          : ExampleDeployment
  CorrelationId           : 07b3b233-8367-4a0b-b9bc-7aa189065665
  ResourceGroupName       : ExampleGroup
  ProvisioningState       : Succeeded
  ...
  ```

4. 若要查看资源组和存储帐户是否已部署到订阅，请使用：

  ```powershell
  Get-AzureRmResourceGroup -Name ExampleGroup
  Find-AzureRmResource -ResourceGroupNameEquals ExampleGroup
  ```

5. 部署模板时，可以指定 PowerShell 参数作为模板参数。 前面的示例未包括任何模板参数，因此使用了模板中的默认值。 若要部署其他存储帐户，并为存储名称前缀和存储帐户 SKU 提供参数值，请使用：

  ```powershell
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment2 -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json -storageNamePrefix "contoso" -storageSKU "Standard_GRS"
  ```

  现在，资源组中已有两个存储帐户。 

## <a name="azure-cli"></a>Azure CLI

1. 若要安装 Azure CLI，请参阅[安装 Azure CLI 2.0](/cli/azure/install-az-cli2)。

2. 若要快速开始进行部署，请使用以下命令：

  ```azurecli
  az login
  az account set --subscription {subscription-id}

  az group create --name ExampleGroup --location "Central US"
  az group deployment create --name ExampleDeployment --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json
  ```

  仅当要使用非默认订阅时，才需要 `az account set` 命令。 若要查看所有订阅及其 ID，请使用：

  ```azurecli
  az account list
  ```

3. 部署可能需要几分钟才能完成。 完成之后，将看到如下消息：

  ```azurecli
  "provisioningState": "Succeeded",
  ```

4. 若要查看资源组和存储帐户是否已部署到订阅，请使用：

  ```azurecli
  az group show --name ExampleGroup
  az resource list --resource-group ExampleGroup
  ```

5. 部署模板时，可以指定 PowerShell 参数作为模板参数。 前面的示例未包括任何模板参数，因此使用了模板中的默认值。 若要部署其他存储帐户，并为存储名称前缀和存储帐户 SKU 提供参数值，请使用：

  ```azurecli
  az group deployment create --name ExampleDeployment2 --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json --parameters '{"storageNamePrefix":{"value":"contoso"},"storageSKU":{"value":"Standard_GRS"}}'
  ```

  现在，资源组中已有两个存储帐户。 

## <a name="example-storage-template"></a>示例存储模板

使用以下示例模板将存储帐户部署到订阅：

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name."
      }
    },
    "storageSKU": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "The type of replication to use for the storage account."
      }
    }
  },
  "variables": {
    "storageName": "[concat(parameters('storageNamePrefix'), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-05-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="next-steps"></a>后续步骤

* 有关使用 PowerShell 部署模板的详细信息，请参阅[使用 Resource Manager 模板和 Azure PowerShell 部署资源](/azure/azure-resource-manager/resource-group-template-deploy)。
* 有关使用 Azure CLI 部署模板的详细信息，请参阅[使用 Resource Manager 模板和 Azure CLI 部署资源](/azure/azure-resource-manager/resource-group-template-deploy-cli)。




