---
title: 使用 Service Fabric 總管視覺化叢集 | Microsoft Docs
description: Service Fabric 總管是一種 Web 型工具，可檢查和管理 Microsoft Azure Service Fabric 叢集中的雲端應用程式和節點。
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: ''

ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2016
ms.author: seanmck

---
# 使用 Service Fabric 總管視覺化叢集
Service Fabric 總管是一個 Web 型工具，可檢查和管理 Azure Service Fabric 叢集中的應用程式與節點。Service Fabric 總管直接裝載於叢集內，因此不論您的叢集在何處執行，它都一律可供使用。

## 連線到 Service Fabric 總管
如果您已依照指示[準備開發環境](service-fabric-get-started.md)，則可以瀏覽至 http://localhost:19080/Explorer 來啟動您本機叢集上的「Service Fabric 總管」。

> [!NOTE]
> 如果您使用 Internet Explorer 搭配 Service Fabric 總管來管理遠端叢集，則需要設定一些 Internet Explorer 設定。若要確保所有資訊都會正確載入，請移至 [工具] > [相容性檢視設定]，然後取消核取 [在相容性檢視下顯示內部網路網站]。
> 
> 

## 了解 Service Fabric 總管配置
您可以使用左邊的樹狀目錄來瀏覽 Service Fabric 總管。在樹狀目錄的根目錄，叢集儀表板會提供您叢集的概觀，包括應用程式和節點健康情況的摘要。

![Service Fabric 總管叢集儀表板][sfx-cluster-dashboard]

### 檢視叢集的配置
Service Fabric 叢集中的節點會橫跨容錯網域和升級網域的二維方格放置。這個位置可確保您的應用程式在發生硬體故障及應用程式升級時仍然可用。您可以使用叢集對應檢視目前叢集的配置方式。

![Service Fabric 總管叢集對應][sfx-cluster-map]

### 檢視應用程式和服務
叢集包含兩個樹狀子目錄：一個用於應用程式，另一個用於節點。

您可以使用應用程式檢視瀏覽 Service Fabric 的邏輯階層：應用程式、服務、資料分割，以及複本。

在以下範例中，應用程式 **MyApp** 是由兩個服務 **MyStatefulService** 與 **WebService** 組成。由於 **MyStatefulService** 可設定狀態，因此它包含一個具有一個主要複本和兩個次要複本的資料分割。對比之下，WebSvcService 則無狀態，而且只包含單一執行個體。

![Service Fabric 總管應用程式檢視][sfx-application-tree]

在樹狀目錄的每個層級，主要窗格會顯示項目的相關資訊。例如，您可以看到特定服務的健康狀態和版本。

![Service Fabric 總管基本資訊窗格][sfx-service-essentials]

### 檢視叢集的節點
節點檢視會顯示叢集的實體配置。對於指定的節點，您可以檢查已經在該節點上部署程式碼的應用程式。更具體地說，您可以看到目前在那裡執行的複本。

## 動作
「Service Fabric 總管」提供一個對您叢集內的節點、應用程式及服務快速叫用動作的方式。

例如，若要刪除某個應用程式執行個體，只要從左邊的樹狀目錄選擇該應用程式，然後選擇 [動作] > [刪除應用程式] 即可。

![在 Service Fabric 總管中刪除應用程式][sfx-delete-application]

> [!TIP]
> 您可以按下每個元素旁邊的省略符號來執行相同動作。
> 
> 

下表列出每個實體的可用動作：

| **實體** | **動作** | **說明** |
| --- | --- | --- |
| 應用程式類型 |解除佈建類型 |從叢集的映像存放區中移除應用程式封裝。需要先移除該類型的所有應用程式。 |
| 應用程式 |刪除應用程式 |刪除應用程式，包括其所有的服務，以及狀態 (如果有的話)。 |
| 服務 |刪除服務 |刪除服務和其狀態 (如果有的話)。 |
| 節點 |啟動 |啟動節點。 |
| 停用 (暫停) |在其目前的狀態中暫停節點。服務會繼續執行，但 Service Fabric 不會主動在節點上移入或移出任何項目，除非有需要這樣做來避免服務中斷或資料不一致的問題。這個動作通常用來在特定節點上啟用偵錯服務，以確保它們不會在檢查期間移動。 | |
| 停用 (重新啟動) |安全地移除所有記憶體內部服務，並關閉持續性服務。通常會在主機處理或電腦必須重新啟動時使用。 | |
| 停用 (移除資料) |在建置足夠的備援複本之後，安全地關閉節點上執行的所有服務。通常於節點 (或至少其儲存體) 永久性地移除委任時使用。 | |
| 移除節點狀態 |從叢集移除節點複本的知識。通常在已失敗的節點被視為無法回復時使用。 | |

由於許多動作都具有破壞性，因此在完成動作之前，系統可能會要求您確認您的意圖。

> [!TIP]
> 可以透過 Service Fabric 總管執行的每個動作也都可以透過 PowerShell 或 REST API 執行，以實現自動化。
> 
> 

您也可以使用 Service Fabric Explorer，針對指定的應用程式類型與版本建立新的應用程式執行個體。在樹狀檢視中選擇應用程式類型，然後在右邊窗格中按一下您想要的版本旁邊的 [建立應用程式執行個體] 連結。

![在 Service Fabric Explorer 中建立應用程式執行個體][sfx-create-app-instance]

> [!NOTE]
> 透過 Service Fabric Explorer 建立的應用程式執行個體目前無法參數化。會使用預設參數值來建立它們。
> 
> 

## 連線至遠端 Service Fabric 叢集
由於「Service Fabric 總管」是 Web 型工具並且是在叢集內執行，因此只要您知道叢集的端點且有足夠的存取權限，便可以從任何瀏覽器存取它。

### 探索遠端叢集的 Service Fabric 總管端點
若要連線到指定之叢集的「Service Fabric 總管」，只需要將您的瀏覽器指向：

http://&lt;your-cluster-endpoint&gt;:19080/Explorer

Azure 入口網站的叢集基本資訊窗格中也會提供完整 URL。

### 連線到安全的叢集
您可以為使用憑證或使用 Azure Active Directory (AAD) 來控制用戶端對您 Service Fabric 叢集的存取。

如果您嘗試連線到安全叢集上的 Service Fabric Explorer，將必須出示用戶端憑證或使用 AAD 登入，需視叢集的組態而定。

## 後續步驟
* [Testability 概觀](service-fabric-testability-overview.md)
* [在 Visual Studio 中管理 Service Fabric 應用程式](service-fabric-manage-application-in-visual-studio.md)
* [使用 PowerShell 部署 Service Fabric 應用程式](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png

<!---HONumber=AcomDC_0824_2016-->