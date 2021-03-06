---
title: 在 OpenSUSE VM 上安裝 MySQL | Microsoft Docs
description: 了解如何在 Azure 中的 OpenSUSE Linux 虛擬機器上安裝 MySQL。
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: timlt
editor: ''
tags: azure-service-management

ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/19/2016
ms.author: cynthn

---
# 在 Azure 中執行 OpenSUSE Linux 的虛擬機器上安裝 MySQL
[MySQL][MySQL] 是一種很受歡迎的開放原始碼 SQL 資料庫。本教學課程會示範如何建立執行 OpenSUSE Linux 的虛擬機器，然後安裝 MySQL。

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

<br>

## 建立執行 OpenSUSE Linux 的虛擬機器
[!INCLUDE [create-and-configure-opensuse-vm-in-portal](../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## 在虛擬機器上安裝和執行 MySQL
[!INCLUDE [install-and-run-mysql-on-opensuse-vm](../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## 後續步驟
如需 MySQL 的詳細資訊，請參閱 [MySQL 文件][MySQLDocs]。

[MySQLDocs]: http://dev.mysql.com/doc/index-topic.html
[MySQL]: http://www.mysql.com

<!---HONumber=AcomDC_0727_2016-->