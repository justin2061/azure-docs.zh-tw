---
title: 在 Azure 資訊安全中心的 SQL Database 上啟用稽核 | Microsoft Docs
description: 本文件說明如何實作 Azure 資訊安全中心建議的「在 SQL Database 上啟用稽核」。
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''

ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/29/2016
ms.author: terrylan

---
# 在 Azure 資訊安全中心的 SQL Database 上啟用稽核
如果尚未啟用稽核，Azure 資訊安全中心將建議您針對所有 SQL Database 開啟稽核。稽核可協助您保持法規遵循、了解資料庫活動，以及深入了解可指出商務考量或疑似安全違規的不一致和異常。

一旦開啟稽核之後，您就可以設定威脅偵測設定和電子郵件來接收安全性警示。威脅偵測會偵測異常資料庫活動，指出資料庫有潛在的安全性威脅。這可讓您在發生潛在威脅時偵測到它們並加以回應。

此建議僅適用於 Azure SQL 服務；不包含在虛擬機器上執行的 SQL。

> [!NOTE]
> 本文件將使用範例部署來介紹服務。這不是逐步指南。
> 
> 

## 實作建議
1. 在 [建議] 刀鋒視窗中，選取 [在 SQL Database 上啟用稽核]。這會開啟 [在 SQL Database 上啟用稽核] 刀鋒視窗。![在 SQL Database 上啟用稽核][1]
2. 選取要在其上啟用稽核的 SQL Database。這會開啟 [稽核與威脅偵測] 刀鋒視窗。![稽核與威脅偵測][2]
3. 在 [稽核與威脅偵測] 刀鋒視窗中，選取 [稽核] 下方的 [開啟]。![開啟稽核與脅偵測][3]
4. 遵循[開始使用 SQL Database 威脅偵測](../sql-database/sql-database-threat-detection-get-started.md)，開啟並設定威脅偵測，以及設定將在偵測到異常活動時接收到安全性警示的電子郵件清單。

## 另請參閱
本文說明了如何實作資訊安全中心建議的「在 SQL Database 上啟用稽核」。 若要深入了解如何保護您的 SQL Database，請參閱下列主題：

* [保護您的 SQL Database](../sql-database/sql-database-security.md)

如要深入了解資訊安全中心，請參閱下列主題：

* [在 Azure 資訊安全中心設定安全性原則](security-center-policies.md) --了解如何為您的 Azure 訂用帳戶及資源群組設定安全性原則。
* [管理 Azure 資訊安全中心的安全性建議](security-center-recommendations.md) -- 了解建議如何協助保護您的 Azure 資源。
* [Azure 資訊安全中心的安全性健全狀況監視](security-center-monitoring.md) -- 了解如何監視 Azure 資源的健全狀況。
* [管理與回應 Azure 資訊安全中心的安全性警示](security-center-managing-and-responding-alerts.md) -- 了解如何管理與回應安全性警示。
* [使用 Azure 資訊安全中心監視合作夥伴解決方案](security-center-partner-solutions.md) -- 了解如何監視合作夥伴解決方案的健康狀態。
* [Azure 資訊安全中心常見問題集](security-center-faq.md) -- 尋找有關使用服務的常見問題。
* [Azure 安全性部落格](http://blogs.msdn.com/b/azuresecurity/) -- 取得最新的 Azure 安全性新聞和資訊。

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection.png
[3]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png

<!---HONumber=AcomDC_0803_2016-->