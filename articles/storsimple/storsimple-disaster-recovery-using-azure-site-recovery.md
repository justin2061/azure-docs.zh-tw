---
title: 使用 Azure Site Recovery 將 StorSimple 上檔案共用的 DR 自動化| Microsoft Docs
description: 說明針對 StorSimple 儲存體上裝載的檔案共用建立災害復原解決方案的步驟與最佳做法。
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: ''

ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/16/2016
ms.author: vidarmsft

---
# 針對 StorSimple 上裝載的檔案共用使用 Azure Site Recovery 的自動化災害復原解決方案
## 概觀
Microsoft Azure StorSimple 是一個混合式雲端儲存體解決方案，可解決通常與檔案共用關聯之非結構化資料的複雜性問題。StorSimple 使用雲端儲存體做為內部部署解決方案的擴充功能，以及跨內部部署儲存體和雲端儲存體自動將資料分層。整合式資料保護 (使用本機和雲端快照集) 不需要擴張的儲存體結構。

[Azure Site Recovery](../site-recovery/site-recovery-overview.md) 是以 Azure 為基礎的服務，藉由協調虛擬機器的複寫、容錯移轉及復原，提供災害復原 (DR) 功能。Azure Site Recovery 支援多種複寫技術來持續複寫、保護及順暢地容錯移轉虛擬機器和應用程式至私密/公用或主機服務提供者的雲端。

您可以使用 Azure Site Recovery、虛擬機器複寫及 StorSimple 雲端快照集功能，來保護完整的檔案伺服器環境。發生中斷時，您可以在 Azure 中使用單鍵復原功能讓您的檔案系統在數分鐘內重新上線。

本文件詳細說明如何為您 StorSimple 儲存體上裝載的檔案共用建立災害復原解決方案，以及使用單鍵復原計劃執行已計劃、未計劃及測試容錯移轉。基本上，它會說明您能夠如何修改 Azure Site Recovery 保存庫中的復原計劃，以在發生災害的情況下啟用 StorSimple 容錯移轉。此外，它也會說明支援的組態與先決條件。本文件假設您已經熟悉 Azure Site Recovery 和 StorSimple 結構的基本知識。

## 支援的 Azure Site Recovery 部署選項
客戶可以將檔案伺服器部署為在 Hyper-V 或 VMware 上執行的實體服務或虛擬機器 (VM)，然後從由 StorSimple 儲存體劃分出來的磁碟區建立檔案共用。Azure Site Recovery 可以保護次要站台或 Azure 的實體與虛擬部署。本文件涵蓋 DR 解決方案的詳細資料，該解決方案使用 Azure 做為 Hyper-V 上裝載之檔案伺服器 VM 的復原網站，並且在 StorSimple 儲存體上使用檔案共用。檔案伺服器 VM 位於 VMware VM 或實體電腦上的其他案例也可以透過類似方式實作。

## 必要條件
實作單鍵災害復原解決方案 (針對 StorSimple 儲存體上裝載的檔案共用使用 Azure Site Recovery) 時，有下列先決條件：

* Hyper-V 或 VMware 或實體電腦上裝載的內部部署 Windows Server 2012 R2 檔案伺服器 VM
* 在 Azure StorSimple Manager 註冊之 StorSimple 儲存體裝置內部部署
* 在 Azure StorSimple Manager (這可以保持在關機狀態) 中建立的 StorSimple Cloud Appliance
* 裝載在 StorSimple 儲存體裝置上設定之磁碟區中的檔案共用
* 在 Microsoft Azure 訂用帳戶中建立的 [Azure Site Recovery 服務保存庫](../site-recovery/site-recovery-vmm-to-vmm.md)

此外，如果 Azure 是您的復原網站，請在 VM 上執行 [Azure 虛擬機器整備評估工具](http://azure.microsoft.com/downloads/vm-readiness-assessment/)，以確保相容於 Azure VM 與 Azure Site Recovery 服務。

為避免延遲問題 (可能導致較高成本)，請確定您是在相同區域中建立 StorSimple Cloud Appliance、自動化帳戶及儲存體帳戶。

## 針對 StorSimple 檔案共用啟用 DR
內部部署環境的每個元件都需要受到保護，才能做到完整的複寫與復原。本節說明如何：

* 設定 Active Directory 和 DNS 複寫 (選擇性)
* 使用 Azure Site Recovery 來保護檔案伺服器 VM
* 保護 StorSimple 磁碟區
* 設定網路

### 設定 Active Directory 和 DNS 複寫 (選擇性)
如果您想要保護執行 Active Directory 和 DNS 的電腦，讓它們能夠在 DR 網站上提供使用，您需要明確地保護它們 (檔案伺服器就能在容錯移轉之後使用驗證來存取)。根據客戶內部部署環境的複雜度而定，有兩個建議的選項。

#### 選項 1
如果客戶的整個內部部署網站中只有少數的應用程式和單一網域控制站，且將容錯移轉整個網站，我們建議使用 Azure Site Recovery 複寫將網域控制站電腦複寫至次要網站 (適用於網站對網站和網站對 Azure)。

#### 選項 2
如果客戶有大量應用程式、正在執行 Active Directory 樹系，且一次將只容錯移轉少數應用程式，我們建議在 DR 網站 (次要網站或 Azure 中) 另外設定一個網域控制站。

請參閱[使用 Azure Site Recovery 自動化 Active Directory 與 DNS 的 DR 解決方案](../site-recovery/site-recovery-active-directory.md)，以取得在 DR 網站上提供網域控制站的指示。對於本文件的其餘部分，我們將假設 DR 網站上有網域控制站可用。

### 使用 Azure Site Recovery 來保護檔案伺服器 VM
此網站需要您準備內部部署伺服器環境、建立及準備 Azure Site Recovery 保存庫，以及啟用 VM 的檔案保護。

#### 保護內部部署檔案伺服器環境
1. 將 [使用者帳戶控制] 設為 [一律不通知]。這是必要的，以便您可以在透過 Azure Site Recovery 容錯移轉之後使用 Azure 自動化指令碼連線 iSCSI。
   
   1. 按 Windows 鍵 +Q 並搜尋 **UAC**。
   2. 選取 [變更使用者帳戶控制設定]。
   3. 將橫條向底部的 [一律不通知] 方向拖曳。
   4. 按一下 [確定] 然後在提示時選取 [是]。
      
      ![](./media/storsimple-dr-using-asr/image1.png)
2. 在每部檔案伺服器 VM 上安裝 VM 代理程式。這是必要的，以便您可以在已容錯移轉的 VM 上執行 Azure 自動化指令碼。
   
   1. [下載代理程式](http://aka.ms/vmagentwin)至`C:\\Users\<username>\\Downloads`
   2. 以系統管理員模式 (以系統管理員身分執行) 開啟 Windows PowerShell，然後輸入下列命令以瀏覽至下載位置：
      
      `cd C:\\Users\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`
      
      > [!NOTE]
      > 視版本不同，檔案名稱可能會改變。
      > 
      > 
3. 按 [下一步]。
4. 接受 [合約條款] 然後按一下 [下一步]。
5. 按一下 [完成]。
6. 使用從 StorSimple 儲存體劃分出來的磁碟區建立檔案共用。如需詳細資訊，請參閱[使用 StorSimple Manager 服務管理磁碟區](storsimple-manage-volumes.md)。
   
   1. 在您的內部部署 VM 上，按 Windows 鍵 +Q 並搜尋 **iSCSI**。
   2. 選取 [iSCCI 啟動器]。
   3. 選取 [組態] 索引標籤並複製啟動器名稱。
   4. 登入 [Azure 傳統入口網站](https://manage.windowsazure.com/)。
   5. 選取 [StorSimple] 索引標籤，然後選取包含實體裝置的 StorSimple Manager 服務。
   6. 建立磁碟區容器，然後建立磁碟區。(這些磁碟區是供檔案伺服器 VM 上的檔案共用使用)。在您建立磁碟區時，請複製啟動器名稱並為「存取控制記錄」提供適當名稱。
   7. 選取 [組態] 索引標籤並記下裝置的 IP 位址。
   8. 在您的內部部署 VM 上，再度移到 [iSCSI 啟動器]，並在 [快速連線] 區段中輸入 IP。按一下 [快速連線]\(裝置現在應該就會連線)。
   9. 開啟 Azure 管理入口網站，然後選取 [磁碟區和裝置] 索引標籤。按一下 [自動設定]。您剛剛建立的磁碟區應該就會出現。
   10. 在入口網站中，選取 [裝置] 索引標籤，然後選取 [建立新的虛擬裝置]。 (此虛擬裝置將會在發生容錯移轉時使用)。這個新的虛擬裝置可以維持在離線狀態以避免產生額外成本。若要讓虛擬機器離線，請移至入口網站的 [虛擬機器] 區段並將它關閉。
   11. 回到內部部署 VM並開啟磁碟管理 (按 Windows 鍵 + X 並選取 [磁碟管理])。
   12. 您將會發現一些額外的磁碟 (視您已經建立的磁碟區數目而定)。在第一個磁碟上按一下滑鼠右鍵，選取 [初始化磁碟]，然後選取 [確定]。在 [未配置] 區段中按一下滑鼠右鍵，選取 [新增簡單磁碟區]，指派一個磁碟機代號給它，然後結束精靈。
   13. 為所有磁碟重複步驟 I。您現在可以在 Windows 檔案總管中的 [這部電腦] 上看到所有磁碟。
   14. 請使用檔案和存放服務角色在這些磁碟區上建立檔案共用。

#### 建立及準備 Azure Site Recovery 保存庫
請參閱 [Azure Site Recovery 文件](../site-recovery/site-recovery-hyper-v-site-to-azure.md)來於保護檔案伺服器 VM 之前開始使用 Azure Site Recovery。

#### 啟用保護
1. 透過 Azure Site Recovery 將 iSCSI 目標與您想要保護的內部部署 VM 中斷連線：
   
   1. 按 Windows 鍵 +Q 並搜尋 **iSCSI**。
   2. 選取 [設定 iSCSI 啟動器]。
   3. 中斷連線您之前連線的 StorSimple 裝置。或者，您可以在啟用保護時將檔案伺服器關閉幾分鐘。
   
   > [!NOTE]
   > 這將會導致檔案共用暫時無法使用
   > 
   > 
2. 從 Azure Site Recovery 入口網站為檔案伺服器 VM [啟用虛擬機器保護](../site-recovery/site-recovery-hyper-v-site-to-azure.md##step-6-enable-replication)。
3. 當起始同步處理開始時，您可以再次重新連線目標。移至 iSCSI 啟動器，選取 StorSimple 裝置，然後按一下 [連線]。
4. 當同步完成且 VM 的狀態為 [受保護] 時，請選取 VM，選取 [設定] 索引標籤，然後據以更新 VM 的網路 (這是已容錯移轉的 VM 所屬的網路)。如果網路沒有顯示，表示同步仍在進行中。

### 保護 StorSimple 磁碟區
如果您尚未選取 StorSimple 磁碟區的 [啟用此磁碟區的預設備份] 選項，請移至 StorSimple Manager 服務中的 [備份原則]，然後為所有磁碟區建立適當的備份原則。我們建議您將備份頻率設定為您要查看應用程式的復原點目標 (RPO)。

### 設定網路
對於檔案伺服器 VM，請設定 Azure Site Recovery 中的網路設定，讓 VM 網路能夠在容錯移轉之後連結到正確的 DR 網路。

您可以在 [VMM 雲端] 或 [保護群組] 中選取 VM 以設定網路設定，如以下圖例所示。

![](./media/storsimple-dr-using-asr/image2.png)

## 建立復原計畫
您可以在 ASR 中建立復原計劃來將檔案共用的容錯移轉程序自動化。如果發生中斷，您只要按一下就可以在幾分鐘內讓檔案共用重新上線。若要啟用此自動化功能，您需要 Azure 自動化帳戶。

#### 建立帳戶
1. 移至 Azure 傳統入口網站並移至 [自動化] 區段。
2. 建立新的自動化帳戶。請將它維持在 StorSimple Cloud Appliance 和儲存體帳戶建立所在的相同地理區域中。
3. 按一下 [新增] &gt; [應用程式服務] &gt; [自動化] &gt; [Runbook] &gt; [從資源庫]，來將所有必要的 Runbook 匯入自動化帳戶。
   
   ![](./media/storsimple-dr-using-asr/image3.png)
4. 從資源庫中的 [災害復原] 窗格新增下列 Runbook：
   
   * 容錯移轉 StorSimple 磁碟區容器
   * 在測試容錯移轉 (TFO) 之後清除 StorSimple 磁碟區
   * 在容錯移轉之後於 StorSimple 裝置上掛接磁碟區
   * 啟動 StorSimple 虛擬設備
   * 在 Azure VM 中解除安裝自訂指令碼擴充功能
     
     ![](./media/storsimple-dr-using-asr/image4.png)
5. 透過選取自動化帳戶中的 Runbook 並移至 [作者] 索引標籤來發佈所有指令碼。在這個步驟之後，[Runbook] 索引標籤看起來會像下面這樣：
   
    ![](./media/storsimple-dr-using-asr/image5.png)
6. 在自動化帳戶中，移至 [資產] 索引標籤，按一下 [加入設定] &gt; [加入認證]，然後加入您的 Azure 認證 – 將資產命名為 AzureCredential。
   
   使用 Windows PowerShell 認證。這應該是包含組織識別碼使用者名稱與密碼的認證，具備此 Azure 訂用帳戶的存取權且已經停用多重要素驗證。這是必要的，可在容錯移轉期間代表使用者進行驗證，並讓 DR 網站上的檔案伺服器磁碟區上線。
7. 在在自動化帳戶中，選取 [資產] 索引標籤，然後按一下 [加入設定] &gt; [加入變數]，然後加入下列變數。您可以選擇將這些資產加密。這些變數都是復原計劃特定變數。如果您的復原計劃 (您將會在下一個步驟中建立) 名稱為 TestPlan，您的變數就應該是 TestPlan-StorSimRegKey、TestPlan-AzureSubscriptionName 等等。
   
   * *RecoveryPlanName***-StorSimRegKey**：StorSimple Manager 服務的註冊金鑰。
   * *RecoveryPlanName***-AzureSubscriptionName**：Azure 訂用帳戶的名稱
   * *RecoveryPlanName***-ResourceName**：擁有 StorSimple 裝置的 StorSimple 資源的名稱。
   * *RecoveryPlanName***-DeviceName**：必須容錯移轉的裝置。
   * *RecoveryPlanName***-TargetDeviceName**：容器容錯移轉之後所在的 StorSimple Cloud Appliance。
   * *RecoveryPlanName***-VolumeContainers**：需容錯移轉之裝置上顯示的磁碟區容器字串 (以逗號區隔)；例如 volcon1、volcon2、volcon3。
   * RecoveryPlanName**-TargetDeviceDnsName**：目標裝置的服務名稱 (這可以在 [虛擬機器] 區段中找到：服務名稱與 DNS 名稱相同)。
   * *RecoveryPlanName***-StorageAccountName**：儲存體帳戶名稱，指令碼 (必須在已容錯移轉的 VM 上執行) 將使用此名稱來儲存。這可以是任何擁有一些空間可暫時儲存指令碼的儲存體帳戶。
   * *RecoveryPlanName***-StorageAccountKey**：上述儲存體帳戶的存取金鑰。
   * *RecoveryPlanName***-ScriptContainer**：將儲存指令碼之雲端容器的名稱。如果容器不存在，就會建立容器。
   * *RecoveryPlanName***-VMGUIDS**：在保護 VM 時，Azure Site Recovery 會為每個 VM指派一個唯一識別碼，該識別碼可提供已容錯移轉之 VM 的詳細資料。若要取得 VMGUID，請選取 [復原服務] 索引標籤，然後按一下 [受保護的項目] &gt; [保護群組] &gt; [電腦] &gt; [內容]。如果您有多個 VM，請透過以逗號區隔的字串方式新增 GUID。
   * *RecoveryPlanName***-AutomationAccountName** – 您已在其中新增 Runbook 和資產的自動化帳戶的名稱。
   
   例如，如果復原計劃的名稱是 fileServerpredayRP，您的 [資產] 索引標籤就應在您新增所有資產之後顯示如下。
   
   ![](./media/storsimple-dr-using-asr/image6.png)
8. 移至 [復原服務] 區段並選取您之前建立的 Azure Site Recovery 保存庫。
9. 選取 [復原計劃] 索引標籤並建立新的復原計劃，如下所示：
   
   a.指定名稱並選取適當的 [保護群組]。
   
   b.從保護群組中選取您要在復原計劃中包含的 VM。
   
   c.建立復原計劃之後，選取該復原計劃來開啟復原計劃自訂檢視。
   
   d.選取 [所有關閉的群組]，按一下 [指令碼]，然後選擇 [在所有關閉的群組前面加入主要端指令碼]。
   
   e.選取自動化帳戶 (您在其中新增 Runbook 的帳戶)，然後選取 [Fail over-StorSimple-Volume-Containers] Runbook。
   
   f.按一下 [群組 1：開始]，選擇 [虛擬機器]，然後新增要在復原計劃中保護的 VM。
   
   g.按一下 [群組 1：開始]，選擇 [指令碼]，然後依照 [群組 1 之後] 步驟的順序新增以下所有指令碼。
   
   * Start-StorSimple-Virtual-Appliance runbook
   * Fail over-StorSimple-volume-containers runbook
   * Mount-volumes-after-failover runbook
   * Uninstall-custom-script-extension runbook
10. 在相同的 [群組 1：後續步驟] 區段中，於上面 4 個指令碼之後，加入一個手動動作。這個動作是您可以確認所有項目都正確運作的點。只有在做為測試容錯移轉的一部份時，才需要新增這個動作 (所以只選取 [測試容錯移轉] 核取方塊)。
11. 在執行手動動作之後，請使用您為其他 Runbook 使用的相同程序新增 Cleanup 指令碼。儲存復原計畫
    
    > [!NOTE]
    > 執行測試容錯移轉時，您應該在手動動作步驟中確認所有項目，因為已在目標裝置上複製的 StorSimple 磁碟區，將在手動動作完成之後於清除作業中一併刪除。
    > 
    > 
    
    ![](./media/storsimple-dr-using-asr/image7.png)

## 執行測試容錯移轉
請參閱 [Active Directory DR 解決方案](../site-recovery/site-recovery-active-directory.md)附屬指南，以了解測試容錯移轉期間 Active Directory 的特定考量。執行測試容錯移轉時，並不會干擾內部部署設定。已連結至內部部署 VM 的 StorSimple 磁碟區會複製到 Azure 上的 StorSimple Cloud Appliance。Azure 中用於測試用途的 VM 會啟動，且雲端磁碟區也會連結至 VM。

#### 執行測試容錯移轉
1. 在 Azure 傳統入口網站中，選取您的站台復原保存庫。
2. 按一下為檔案伺服器 VM 建立的復原計劃。
3. 按一下 [測試容錯移轉]。
4. 選取虛擬網路來開始測試容錯移轉程序。
   
   ![](./media/storsimple-dr-using-asr/image8.png)
5. 當次要環境啟動時，您就可以執行您的驗證。
6. 驗證完成之後，按一下 [驗證完成]。將會清除測試容錯移轉環境，且將會完成 TFO 作業。

## 執行非計劃性容錯移轉
在非計劃性容錯移轉期間，StorSimple 磁碟區會容錯移轉至虛擬裝置，複本 VM 將在 Azure 上啟動，且磁碟區會連結至 VM。

#### 執行非計劃性容錯移轉
1. 在 Azure 傳統入口網站中，選取您的站台復原保存庫。
2. 按一下為檔案伺服器 VM 建立的復原計劃。
3. 按一下 [容錯移轉]，然後選取 [非計劃性容錯移轉]。
   
   ![](./media/storsimple-dr-using-asr/image9.png)
4. 選取目標網路，然後按一下核取圖示 ✓ 來開始容錯移轉程序。

## 執行計劃性容錯移轉
在計劃性容錯移轉期間，內部部署檔案伺服器 VM 會正常關機，且會使用 StorSimple 裝置上磁碟區的雲端備份快照集。StorSimple 磁碟區會容錯移轉至虛擬裝置，複本 VM 會在 Azure 上啟動，且磁碟區會連結至 VM。

#### 執行計劃性容錯移轉
1. 在 Azure 傳統入口網站中，選取您的站台復原保存庫。
2. 按一下為檔案伺服器 VM 建立的復原計劃。
3. 按一下 [容錯移轉]，然後選取 [計劃性容錯移轉]。
4. 選取目標網路，然後按一下核取圖示 ✓ 來開始容錯移轉程序。

## 執行容錯移轉
在容錯移轉期間，StorSimple 磁碟區容器會在使用備份之後容錯移轉回實體裝置。

#### 執行容錯移轉
1. 在 Azure 傳統入口網站中，選取您的站台復原保存庫。
2. 按一下為檔案伺服器 VM 建立的復原計劃。
3. 按一下 [容錯移轉] 並選取 [計劃性容錯移轉] 或 [非計劃性容錯移轉]。
4. 按一下 [變更方向]。
5. 選取適當的資料同步處理和 VM 建立選項。
6. 按一下核取圖示 ✓ 以開始容錯移轉程序。
   
   ![](./media/storsimple-dr-using-asr/image10.png)

## 最佳作法
### 容量規劃和整備性評估
#### Hyper-V 站台
使用[使用者容量規劃工具](http://www.microsoft.com/download/details.aspx?id=39057)來為您的 Hyper-V 複本環境設計伺服器、儲存體及網路基礎結構。

#### Azure
您可以在 VM 上執行 [Azure 虛擬機器整備評估工具](http://azure.microsoft.com/downloads/vm-readiness-assessment/)，以確保它們相容於 Azure VM 與 Azure Site Recovery 服務整備評估工具會檢查 VM　組態，並在組態與 Azure 不相容時提出警告。例如，如果 C: 磁碟大小超過 127 GB，它就會提出警告。.

容量規劃至少包含兩個重要程序：

* 將內部部署 Hyper-V VM 對應至 Azure VM 大小 (例如 A6、A7、A8 及 A9)。
* 判斷所需的網際網路頻寬。

## 限制
* 目前只有 1 部 StorSimple 裝置可以容錯移轉 (至單一 StorSimple Cloud Appliance)。尚不支援跨越數部 StorSimple 裝置的檔案伺服器案例。
* 如果您在啟用 VM 的保護時發生錯誤，請確定您已經中斷連線 iSCSI 目標。
* 已經因為備份原則跨越不同磁碟區容器而一起組成群組的所有磁碟區容器都會一起容錯移轉。
* 磁碟區容器中您已經選擇的所有磁碟區都會容錯移轉。
* 加總後超過 64 TB 的磁碟區無法容錯移轉，因為單一 StorSimple Cloud Appliance 的容量上限為 64 TB。
* 如果計劃性/非計劃性容錯移轉失敗，且 Azure 中已經建立 VM，此時請勿清除 VM。請改為執行容錯回復。如果您刪除 VM，就會無法再次開啟內部部署 VM。
* 在容錯移轉之後，如果您看不到磁碟區，請移至 VM，開啟「磁碟管理」，重新掃描磁碟，然後讓磁碟上線。
* 在某些情況下，DR 網站中的磁碟機代號可能和內部部署的代號不同。如果發生此情況，您必須在容錯移轉完成之後手動修正問題。
* 應針對在自動化帳戶中輸入做為資產的 Azure 認證停用多重要素驗證。如果沒有停用此驗證，指令碼將無法自動執行，且復原計劃將會失敗。
* 容錯移轉工作逾時：如果容錯移轉磁碟區容器時花費的時間超出每個指令碼的 Azure Site Recovery 限制 (目前為 120 分鐘)，StorSimple 指令碼將會逾時。
* 備份工作逾時：如果備份磁碟區時花費的時間超出每個指令碼的 Azure Site Recovery 限制 (目前為 120 分鐘)，StorSimple 指令碼就會逾時。
  
  > [!IMPORTANT]
  > 請從 Azure 入口網站手動執行備份，然後再執行一次復原計劃。
  > 
  > 
* 複製工作逾時：如果複製磁碟區時花費的時間超出每個指令碼的 Azure Site Recovery 限制 (目前為 120 分鐘)，StorSimple 指令碼就會逾時。
* 時間同步處理錯誤：StorSimple 指令碼發生錯誤並指出備份失敗，即使在入口網站中備份成功。發生此問題的可能原因或許是因為 StorSimple 設備的時間可能沒有和時區中目前的時間同步。
  
  > [!IMPORTANT]
  > 請將設備時間與時區中目前的時間同步。
  > 
  > 
* 設備容錯移轉錯誤：如果復原計劃執行時有進行設備容錯移轉，StorSimple 指令碼可能會失敗。
  
  > [!IMPORTANT]
  > 請在設備容錯移轉完成後重新執行復原計劃。
  > 
  > 

## 摘要
使用 Azure Site Recovery 時，您可以為有檔案共用裝載於 StorSimple 儲存體上的檔案伺服器 VM 建立完整自動化的災害復原計劃。當發生中斷時，您可以在幾秒鐘內從任何地方起始容錯移轉，並且在數分鐘內啟動並執行應用程式。

<!---HONumber=AcomDC_0518_2016-->