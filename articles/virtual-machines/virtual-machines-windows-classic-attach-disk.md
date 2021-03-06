---
title: 將磁碟附加至 VM | Microsoft Docs
description: 將資料磁碟連接到以傳統部署模型建立的 Windows 虛擬機器，並予以初始化。
services: virtual-machines-windows, storage
documentationcenter: ''
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management

ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/27/2016
ms.author: cynthn

---
# <a name="attach-a-data-disk-to-a-windows-virtual-machine-created-with-the-classic-deployment-model"></a>將資料磁碟連接至以傳統部署模型建立的 Windows 虛擬機器
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

如果您想要使用新的入口網站，請參閱 [如何在 Azure 入口網站中將資料磁碟連結到 Windows VM](virtual-machines-windows-attach-disk-portal.md)。

如果您需要其他資料磁碟，可將空磁碟或現有的資料磁碟連接到 VM。 在這兩種情況下，磁碟是位於 Azure 儲存體帳戶中的 .vhd 檔案。 如果是新的磁碟，則在連接磁碟之後，您也需要將它初始化，使其可供 Windows VM 使用。

如需有關磁碟的詳細資訊，請參閱 [關於虛擬機器的磁碟和 VHD](virtual-machines-windows-about-disks-vhds.md)。

[!INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

## <a name="initialize-the-disk"></a>初始化磁碟
1. 連接至虛擬機器。 如需相關指示，請參閱[如何登入執行 Windows Server 的虛擬機器][登入]。
2. 登入虛擬機器之後，開啟 [伺服器管理員] 。 在左窗格中，選取 [File and Storage Services] 。
   
    ![開啟伺服器管理員](./media/virtual-machines-windows-classic-attach-disk/fileandstorageservices.png)
3. 展開功能表，然後選取 [ **磁碟**]。
4. [磁碟]  區段會列出磁碟。 在大部分情況下，會有磁碟 0、磁碟 1 和磁碟 2。 磁碟 0 是作業系統磁碟、磁碟 1 是暫存磁碟，磁碟 2 則是您剛連接至 VM 的資料磁碟。 新的資料磁碟會將磁碟分割列為 [未知] 。 在磁碟上按一下滑鼠右鍵，然後選取 [初始化] 。
5. 初始化磁碟時，您會收到將清除所有資料的通知。 按一下 [ **是** ]，認可此警告並初始化磁碟。 完成之後，即會將磁碟分割列為 [GPT] 。 再次於磁碟上按一下滑鼠右鍵，然後選取 [新增磁碟區] 。
6. 使用預設值完成精靈。 當精靈完成時，[ **磁碟區** ] 區段會列出新的磁碟區。 磁碟此時將上線，可用來儲存資料。
   
   ![成功初始化磁碟區](./media/virtual-machines-windows-classic-attach-disk/newvolumecreated.png)

> [!NOTE]
> VM 的大小會決定它可以附加的磁碟數目。 如需詳細資訊，請參閱 [虛擬機器的大小](virtual-machines-linux-sizes.md)。
> 
> 

## <a name="additional-resources"></a>其他資源
[如何從 Windows 虛擬機器卸離磁碟](virtual-machines-windows-classic-detach-disk.md)

[有關虛擬機器的磁碟和 VHD](virtual-machines-linux-about-disks-vhds.md)

[登入]: virtual-machines-windows-classic-connect-logon.md



<!--HONumber=Oct16_HO2-->


