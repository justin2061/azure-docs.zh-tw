---
title: Windows Phone Silverlight SDK 概觀
description: 適用於 Azure Mobile Engagement 的 Windows Phone Silverlight SDK 概觀
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: dwrede
editor: ''

ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo

---
# 適用於 Azure Mobile Engagement 的 Windows Phone Silverlight SDK 概觀
從這裡開始了解在 Windows Phone Silverlight 應用程式中整合 Azure Mobile Engagement 的細節。如果您想要先試用一下，請務必先完成我們的 [15 分鐘教學課程](mobile-engagement-windows-phone-get-started.md)。

按一下以查看 [SDK 內容](mobile-engagement-windows-phone-sdk-content.md)

## 整合程序
1. 從這裡開始：[如何在 Windows Phone Silverlight 應用程式中整合 Mobile Engagement](mobile-engagement-windows-phone-integrate-engagement.md)
2. 通知：[如何在 Windows Phone Silverlight 應用程式中整合 Reach (通知)](mobile-engagement-windows-phone-integrate-engagement-reach.md)
3. 標記計劃實作：[如何在 Windows Phone Silverlight 應用程式中使用進階的 Mobile Engagement 標記 API](mobile-engagement-windows-phone-use-engagement-api.md)。

## 版本資訊
### 3\.3.0 (04/19/2016)
*MicrosoftAzure.MobileEngagement* Nuget 封裝 **v3.4.0** 的一部分

* 已加入 "TestLogLevel" API 來啟用/停用/篩選 SDK 所發出的主控台記錄檔。

如需較早版本，請參閱[完整版本資訊](mobile-engagement-windows-phone-release-notes.md)

## 升級程序
如果您已經整合我們的舊版 SDK 到您的應用程式，在升級 SDK 時您必須考慮以下幾點。

如果您有錯過幾個版本的 SDK，您必須遵循幾個步驟。請參閱完整的[升級程序](mobile-engagement-windows-phone-upgrade-procedure.md)。例如，如果您要從 0.10.1 移轉到 0.11.0，必須先遵循「從 0.9.0 到 0.10.1」的程序，然後「從 0.10.1 到 0.11.0」的程序。

### 從 2.0.0 到 3.3.0
#### 測試記錄檔
SDK 所產生的主控台記錄檔現在可以啟用/停用/篩選。若要自訂這種情況，請將屬性 `EngagementAgent.Instance.TestLogEnabled` 更新為 `EngagementTestLogLevel` 列舉的其中一個可用值，例如︰

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### 從舊版升級
請參閱[升級程序](mobile-engagement-windows-phone-upgrade-procedure.md)

<!---HONumber=AcomDC_0824_2016-->