---
title: 使用 Azure AD 應用程式 Proxy 連接器 | Microsoft Docs
description: 涵蓋如何建立和管理「Azure AD 應用程式 Proxy」中的連接器群組。
services: active-directory
documentationcenter: ''
author: kgremban
manager: femila
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/09/2016
ms.author: kgremban

---
# 使用連接器群組在個別的網路和位置上發佈應用程式
> [!div class="op_single_selector"]
> * [Azure 入口網站](active-directory-application-proxy-connectors-azure-portal.md)
> * [Azure 傳統入口網站](active-directory-application-proxy-connectors.md)
> 
> 

連接器群組可用於多種不同狀況，包括：

* 具有多個互連資料中心的網站。在此情況下，您希望資料中心內儘可能保持最大流量，因為跨資料中心的連結通常昂貴且緩慢。您可以在每個資料中心都部署連接器，來只為資料中心內的應用程式提供服務。這種方法可將跨資料中心的連結減到最少，讓使用者體驗完全的流暢性。
* 管理安裝在隔離網路 (不是主要的公司網路的一部分) 上的應用程式。您可以使用連接器群組將專用連接器安裝在隔離網路上，以同時將應用程式與網路隔離。
* 對於安裝在 IaaS 上以供雲端存取的應用程式，連接器群組提供通用的服務來保護對所有應用程式的存取。連接器群組不會對公司網路產生額外的相依性，或是造成不完整的應用程式體驗。連接器可安裝在每個雲端資料中心，而且只為此網路中的應用程式提供服務。您可以安裝數個連接器以獲得高可用性。
* 支援多樹系環境，其中可以依樹系部署特定連接器，並設定為針對特定應用程式提供服務。
* 連接器群組可用於災害復原網站，以偵測容錯移轉或做為主要網站的備份。
* 連接器群組也可用來為單一租用戶的多家公司提供服務。

## 必要條件︰建立連接器
為了將連接器組成群組，您必須確定[已安裝多個連接器](active-directory-application-proxy-enable.md)並指定它們的名稱，然後再將它們組成群組。最後，您必須將它們指派給特定的應用程式。

## 步驟 1：建立連接器群組
您可以建立任意數量的連接器群組。連接器群組的建立是在 Azure 傳統入口網站中完成。

1. 選取您的目錄，然後按一下 [設定]。![應用程式 Proxy 設定的螢幕擷取畫面 - 按一下 [管理連接器群組]](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)
2. 在 [應用程式 Proxy] 底下，按一下 [管理連接器群組]，然後提供一個群組名稱來建立新的連接器群組。![應用程式 Proxy 連接器群組的螢幕擷取畫面 - 命名新的群組](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)

## 步驟 2：將連接器指派給您的群組
建立連接器群組之後，請將連接器移到適當的群組。

1. 在 [應用程式 Proxy] 底下，按一下 [管理連接器]。
2. 在 [群組] 底下，選取要用於每個連接器的群組。連接器可能最多需要 10 分鐘才會在新群組中變成作用中。![應用程式 Proxy 連接器螢幕擷取畫面 - 從下拉式功能表中選取群組](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)

## 步驟 3：將應用程式指派給您的連接器群組
最後一個步驟是將每個應用程式設定至將為其提供服務的連接器群組。

1. 在 Azure 傳統入口網站中，於您的目錄中選取要指派給群組的應用程式，然後按一下 [設定]。
2. 在 [連接器群組] 底下，選取應用程式要使用的群組。這項變更會立即套用。![應用程式 Proxy 連接器群組螢幕擷取畫面 - 從下拉式功能表中選取群組](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)

## 另請參閱
* [啟用應用程式 Proxy](active-directory-application-proxy-enable.md)
* [啟用單一登入](active-directory-application-proxy-sso-using-kcd.md)
* [啟用條件式存取](active-directory-application-proxy-conditional-access.md)
* [使用應用程式 Proxy 疑難排解您遇到的問題](active-directory-application-proxy-troubleshoot.md)

如需最新消息，請查閱[應用程式 Proxy 部落格](http://blogs.technet.com/b/applicationproxyblog/)

<!---HONumber=AcomDC_0914_2016-->