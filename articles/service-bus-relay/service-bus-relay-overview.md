---
title: "服務匯流排轉送概觀 | Microsoft Docs"
description: "服務匯流排轉送概觀。"
services: service-bus
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1038a2d8-5def-4f48-8703-cb0070fc5f10
ms.service: service-bus
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0482096cbec6a5e4b7b13ea662a180cd9b96e85f


---
# <a name="overview-of-service-bus-relay"></a>服務匯流排轉送概觀
服務匯流排的主要元件是一種集中式 (但高度負載平衡)「轉送」服務，能讓您建立一個可在 Azure 資料中心和您自己的內部部署企業環境中執行的混合式應用程式。  服務匯流排轉送支援各種不同的傳輸通訊協定和 Web 服務標準。 這包括 SOAP、WS-*，甚至是 REST。 轉送服務可執行您的混合式應用程式，方法是讓您以安全的方式，向公用雲端公開位於企業網路內部的 Windows Communication Foundation (WCF) 服務，而無需開啟防火牆連線或要求對企業網路基礎結構的進行侵入式變更。 

![WCF 轉送概念](./media/service-bus-relay-overview/sb-relay-01.png)

轉送服務支援傳統的單向訊息、要求/回應訊息，以及對等式訊息。 它也支援網際網路範圍內的事件散發，以啟用發佈/訂閱案例和雙向通訊端通訊來提高點對點效率。 

在轉送傳訊模式中，內部部署服務會透過輸出連接埠連接到轉送服務，並且針對繫結至特定聚合位址的通訊建立雙向通訊端。 用戶端接著可將訊息傳送至以會合位址為目標的轉送服務，藉此與內部部署服務通訊。 轉送服務接著會透過已就緒的雙向通訊端，將訊息「轉送」至內部部署服務。 用戶端不需要直接連線到內部部署服務，不需要知道服務所在的位置，而且內部部署服務不需要在防火牆上開啟任何輸入連接埠。

您在內部部署服務與使用一組 WCF「轉送」繫結的轉送服務之間起始連線。 在幕後，轉送繫結會對應至新的傳輸繫結元素，其設計來建立與雲端中服務匯流排整合的 WCF 通道元件。 

## <a name="next-steps"></a>後續步驟
如需服務匯流排轉送的詳細資訊，請參閱下列主題。

* [Azure 服務匯流排架構概觀](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* [如何使用服務匯流排 WCF 轉送服務](service-bus-dotnet-how-to-use-relay.md)




<!--HONumber=Nov16_HO2-->


