---
title: 設定 Azure 傳統入口網站中的 VPN 閘道 | Microsoft Docs
description: 本文將逐步引導您設定虛擬網路 VPN 閘道，以及變更閘道 VPN 路由類型。
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: ''
tags: azure-service-management

ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/11/2016
ms.author: cherylmc

---
# 設定傳統部署模型的 VPN 閘道
如果您想要在 Azure 和內部部署位置之間建立安全的跨單位連線，您需要設定 VPN 閘道連線。在傳統部署模型中，閘道可以是兩種 VPN 路由類型的其中一種：靜態或動態。您所選擇的類型取決於您的網路設計規劃，以及您要使用的內部部署 VPN 裝置。

例如，某些連線選項，例如點對站台連接，則需要動態路由閘道。如果您想要將閘道設定為同時支援點對站 (P2S) 連線和站對站 (S2S) 連線，則您必須設定動態路由閘道，即使站對站可以使用其中一種閘道 VPN 路由類型進行設定。

此外，您必須確定要用於連線的裝置支援所要建立的 VPN 路由類型。請參閱[關於 VPN 裝置](vpn-gateway-about-vpn-devices.md)。

**本文內容**

本文章是針對使用[傳統入口網站](https://manage.windowsazure.com) (而非 Azure 入口網站) 的傳統部署模型所撰寫。

**關於 Azure 部署模型**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## 組態概觀
下列步驟將逐步引導您在 Azure 傳統入口網站中設定 VPN 閘道。這些步驟適用於使用傳統部署模型所建立的虛擬網路閘道。目前，並非所有的閘道組態設定都可在 Azure 入口網站中使用。當可使用時，我們將會建立一組適用於 Azure 入口網站的新指示。

1. [為您的 VNet 建立 VPN 閘道](#create-a-vpn-gateway)
2. [收集 VPN 裝置組態的資訊](#gather-information-for-your-vpn-device-configuration)
3. [設定 VPN 裝置](#configure-your-vpn-device)
4. [驗證您的區域網路範圍和 VPN 閘道 IP 位址](#verify-your-local-network-ranges-and-vpn-gateway-ip-address)

### 開始之前
設定您的閘道之前，您必須先建立虛擬網路。如需針對跨單位連線建立虛擬網路的步驟，請參閱[設定虛擬網路與站對站 VPN 連線](vpn-gateway-site-to-site-create.md)，或[設定虛擬網路與點對站 VPN 連線](vpn-gateway-point-to-site-create.md)。然後，使用下列步驟來設定 VPN 閘道，並收集您設定 VPN 裝置所需的資訊。

如果您已擁有 VPN 閘道，且想要變更 VPN 路由類型，請參閱[如何變更閘道的 VPN 路由類型](#how-to-change-the-vpn-routing-type-for-your-gateway)。

## 建立 VPN 閘道
1. 在 [Azure 傳統入口網站](https://manage.windowsazure.com)的 [網路] 頁面上，確認您虛擬網路的狀態欄為 [已建立]。
2. 在 [名稱] 欄中，按一下虛擬網路的名稱。
3. 在 [**儀表板**] 頁面上，請注意此 VNet 尚未設定閘道。當您進行設定閘道的步驟時將會看到此狀態。

![閘道未建立](./media/vpn-gateway-configure-vpn-gateway-mp/IC717025.png)

接下來，按一下頁面底部的 [**建立閘道**]。您可以選取 [*靜態路由*] 或 [*動態路由*]。您選取的 VPN 路由類型取決於一些因素。例如，您 VPN 裝置所支援的類型，以及您是否需要支援點對站連線。請參閱[關於虛擬網路連線的 VPN 裝置](vpn-gateway-about-vpn-devices.md)以驗證您所需要的路由類型。一旦建立閘道之後，您必須刪除並重新建立閘道才能變更閘道 VPN 路由類型。當系統提示您確認是否要建立閘道時，請按一下 [**是**]。

![閘道 VPN 路由類型](./media/vpn-gateway-configure-vpn-gateway-mp/IC717026.png)

閘道正在建立時，請注意頁面上的閘道圖形會變更為黃色，並顯示 [*正在建立閘道*]。閘道建立作業最多可能需要花費 45 分鐘。您必須先等候閘道建立完成，才能繼續進行其他組態設定。

![閘道正在建立](./media/vpn-gateway-configure-vpn-gateway-mp/IC717027.png)

當閘道變更為 [*正在連接*] 時，您可以收集 VPN 裝置所需的資訊。

![閘道正在連接](./media/vpn-gateway-configure-vpn-gateway-mp/IC717028.png)

## 收集 VPN 裝置組態的資訊
建立閘道之後，接著收集 VPN 裝置組態的資訊。這項資訊位於虛擬網路的 [**儀表板**] 頁面：

1. **閘道 IP 位址 -** IP 位址可在 [**儀表板**] 頁面中找到。您必須在閘道建立完成之後才能看見 IP。
2. **共用金鑰 -** 按一下畫面底部的 [**管理金鑰**]。按一下金鑰旁邊的圖示以將它複製到剪貼簿，然後貼上並儲存金鑰。此按鈕僅在有單一 S2S VPN 通道時才能正常運作。如果您有多個 S2S VPN 通道，請使用「取得虛擬網路閘道共用金鑰」API 或 PowerShell Cmdlet。

![管理金鑰](./media/vpn-gateway-configure-vpn-gateway-mp/IC717029.png)

## 設定 VPN 裝置
完成上一個步驟之後，您或您的網路管理員將需要設定 VPN 裝置以便建立連線。請參閱＜[關於虛擬網路連線的 VPN 裝置](vpn-gateway-about-vpn-devices.md)＞以取得 VPN 裝置的詳細資訊。

設定 VPN 裝置之後，您可以在 VNet 的 [儀表板] 頁面上檢視更新的連接資訊。

您也可以執行下列任一命令以測試連接：

|  | Cisco ASA | Cisco ISR/ASR | Juniper SSG/ISG | Juniper SRX/J |
| --- | --- | --- | --- | --- |
| **檢查主要模式 SA** |show crypto isakmp sa |show crypto isakmp sa |get ike cookie |show security ike security-association |
| **檢查快速模式 SA** |show crypto ipsec sa |show crypto ipsec sa |get sa |show security ipsec security-association |

## 驗證您的區域網路範圍和 VPN 閘道 IP 位址
### 驗證您的 VPN 閘道 IP 位址
為了讓閘道能正確連接，您 VPN 裝置的 IP 位址必須針對您為跨單位組態所指定的區域網路進行正確設定。一般而言，這會在設定站台對站台設定程序期間進行設定。不過，如果您先前已搭配不同的裝置使用此區域網路，或此區域網路的 IP 位址已變更，請編輯設定以便指定正確的閘道 IP 位址。

1. 若要驗證您的閘道 IP 位址，請在左側的入口網站窗格上按一下 [**網路**]，然後再選取頁面頂端的 [**區域網路**]。您會看到針對每個本機網路所建立的 VPN 閘道位址。若要編輯的 IP 位址，請選取 VNet，然後按一下頁面底部的 [**編輯**]。
2. 在 [**指定本機網路詳細資料**] 頁面上，編輯 IP 位址，然後按一下頁面底部的 [下一步] 箭頭。
3. 在 [**指定位址空間**] 頁面上，按一下右下角的核取記號以儲存設定。

### 驗證您區域網路的位址範圍
若要使正確的流量能夠流經閘道並通往您的內部部署位置，您需要驗證是否已指定每個 IP 位址範圍。每個範圍都必須列在您的 Azure「區域網路」組態中。根據您內部部署位置的網路組態，這可能是一項大工程。針對包含在列出範圍內 IP 位址所繫結的流量，將會透過虛擬網路 VPN 閘道來傳送。您列出的範圍不必是私人範圍，不過您會想要驗證內部部署組態是否能夠接收輸入流量。

若要新增或編輯區域網路的範圍，請使用下列步驟。

1. 若要編輯區域網路的 IP 位址範圍，請在左側的入口網站窗格上按一下 [**網路**]，然後再選取頁面頂端的 [**區域網路**]。在入口網站中，最簡單方法是檢視您在 [**編輯**] 頁面上所列出的範圍。若要查看範圍，請選取 VNet，然後按一下頁面底部的 [**編輯**]。
2. 在 [**指定本機網路詳細資料**] 頁面上，請勿進行任何變更。按一下頁面底部的 [下一步] 箭頭。
3. 在 [**指定位址空間**] 頁面上，您可以變更網路位址空間。然後按一下核取記號以儲存您的組態。

## 如何檢視閘道流量
您可以從虛擬網路 [**儀表板**] 頁面檢視您的閘道和閘道流量。

您可以在 [**儀表板**] 頁面檢視下列項目：

* 流經閘道，包括資料輸入和資料輸出的資料量。
* 針對虛擬網路所指定的 DNS 伺服器名稱。
* 您的閘道與 VPN 裝置之間的連接。
* 用來設定閘道與 VPN 裝置連接的共用金鑰。

## 如何變更閘道的 VPN 路由類型
因為某些連線組態僅適用於特定閘道路由類型，您可能會發現需要變更現有 VPN 閘道的 VPN 路由閘道類型。例如，您可能要將點對站台連接新增至具有靜態閘道的現有站台對站台連線。點對站連線需要動態閘道。這表示若要設定 P2S 連接，您必須將您的閘道 VPN 路由類型從靜態變更為動態。

如果您需要變更閘道 VPN 路由類型，您必須刪除現有的閘道，然後使用新的路由類型建立新的閘道。變更閘道路由類型不需要刪除整個虛擬網路。

在變更閘道 VPN 路由類型之前，請務必驗證 VPN 裝置可支援所要使用的路由類型。若要下載新的路由組態範例，以及檢查 VPN 裝置需求，請參閱＜[關於虛擬網路連線的 VPN 裝置](vpn-gateway-about-vpn-devices.md)＞。

> [!IMPORTANT]
> 當您刪除虛擬網路 VPN 閘道時，將會釋放指派給該閘道的 VIP。當您重新建立閘道時，系統會將新的 VIP 指派給它。
> 
> 

1. **刪除現有的 VPN 閘道。**
   
    在虛擬網路的 [**儀表板**] 頁面上，瀏覽至頁面底部，然後按一下 [**刪除閘道**]。等候閘道已完成刪除的通知。一旦您在畫面上收到閘道已完成刪除的通知，就可以建立新的閘道。
2. **建立新的 VPN 閘道。**
   
    使用位於頁面頂端的程序來建立新的閘道：[建立 VPN 閘道](#create-a-vpn-gateway)。

## 後續步驟
您可以將虛擬機器加入您的虛擬網路。請參閱[如何建立自訂虛擬機器](../virtual-machines/virtual-machines-windows-classic-createportal.md)。

如果您想要設定點對站 VPN 連線，請參閱[設定點對站 VPN 連線](vpn-gateway-point-to-site-create.md)。

<!---HONumber=AcomDC_0817_2016-->