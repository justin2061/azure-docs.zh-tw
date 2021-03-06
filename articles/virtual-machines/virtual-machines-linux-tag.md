---
title: 如何標記 Linux 虛擬機器 | Microsoft Docs
description: 了解如何標記以資源管理員部署模型於 Azure 中所建立的 Linux 虛擬機器。
services: virtual-machines-linux
documentationcenter: ''
author: mmccrory
manager: timlt
editor: tysonn
tags: azure-resource-manager

ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/05/2016
ms.author: memccror

---
# 如何在 Azure 中標記 Linux 虛擬機器
本文說明在 Azure 中透過 Resource Manager 部署模型標記 Linux 虛擬機器的各種不同方式。標記是使用者定義的成對「索引鍵/值」，可直接置於資源或資源群組。Azure 目前對每一個資源和資源群組最多支援 15 個標記。標記可在建立或加入至現有資源時置於資源上。請注意，標記只支援透過 Resource Manager 部署模型建立的資源。

[!INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## 透過 Azure CLI 進行標記
若要開始，請[安裝和設定 Azure CLI](../xplat-cli-azure-resource-manager.md)，並確定您處於 Resource Manager 模式 (`azure config mode arm`)。

您可以使用這個命令來檢視指定之虛擬機器的所有屬性，包括標記：

        azure vm show -g MyResourceGroup -n MyTestVM

若要透過 Azure CLI 新增 VM 標記，您可以搭配使用 `azure vm set` 命令搭配標記參數 **-t**：

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

若要移除所有標記，您可以在 `azure vm set` 命令中使用 **–T** 參數。

        azure vm set – g MyResourceGroup –n MyTestVM -T


既然我們已將標記套用至我們的資源 Azure CLI 和入口網站，就讓我們來看一下使用量詳細資料，以在計費入口網站中查看標記。

[!INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## 後續步驟
* 如需深入了解如何標記您的 Azure 資源，請參閱 [Azure Resource Manager 概觀][Azure Resource Manager 概觀]與[使用標記來組織您的 Azure 資源][使用標記來組織您的 Azure 資源]。
* 如需查看標記如何協助您管理使用 Azure 資源，請參閱[了解 Azure 帳單][了解 Azure 帳單]與[深入了解 Microsoft Azure 資源耗用量][深入了解 Microsoft Azure 資源耗用量]。

[Azure CLI environment]: ./xplat-cli-azure-resource-manager.md
[Azure Resource Manager 概觀]: ../resource-group-overview.md
[使用標記來組織您的 Azure 資源]: ../resource-group-using-tags.md
[了解 Azure 帳單]: ../billing-understand-your-bill.md
[深入了解 Microsoft Azure 資源耗用量]: ../billing-usage-rate-card-overview.md

<!---HONumber=AcomDC_0706_2016-->