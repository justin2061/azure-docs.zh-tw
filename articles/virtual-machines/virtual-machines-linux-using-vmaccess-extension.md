---
title: 使用 VMAccess 擴充功能重設 Azure Linux VM 的存取 | Microsoft Docs
description: 使用 VMAccess 擴充功能重設 Azure Linux VM 的存取。
services: virtual-machines-linux
documentationcenter: ''
author: vlivech
manager: timlt
editor: ''
tags: azure-resource-manager

ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/30/2016
ms.author: v-livech

---
# 管理使用者、SSH，並使用 VMAccess 擴充功能檢查或修復 Azure Linux VM 上的磁碟
本文將說明如何使用 Azure VMAcesss 延伸模組來檢查或修復磁碟、重設使用者存取、管理使用者帳戶或重設 Linux 上的 SSHD 組態。

必要條件︰[Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/)、[SSH 公開金鑰與私密金鑰](virtual-machines-linux-mac-create-ssh-keys.md)和使用 `azure config mode arm` 切換至 Resource Manager 模式的 Azure CLI。

## 快速命令
有兩種方式可在 Linux VM 上使用 VMAccess：

* 使用 Azure CLI 和必要的參數。
* 使用 VMAccess 處理的原始 JSON 檔案並採取行動。

在快速命令區段中，我們會使用 Azure CLI `azure vm reset-access` 方法。在下列命令範例中，將包含 "example" 的值取代為您環境中的值。

## 建立資源群組和 Linux VM
```bash
azure group create resourcegroupexample westus
```

```bash
azure vm quick-create \
-M ~/.ssh/id_rsa.pub \
-u userexample \
-g resourcegroupexample \
-l westus \
-y Linux \
-n debianexamplevm \
-Q Debian
```

## 重設根密碼
重設根密碼：

```bash
azure vm reset-access -g exampleResourceGroup -n exampleVMName -u root -p examplenewPassword
```

## SSH 金鑰重設
若要重設非根使用者的 SSH 金鑰︰

```bash
azure vm reset-access -g exampleResourceGroup -n exampleVMName -u userexample -M ~/.ssh/id_rsa.pub
```

## 建立使用者
若要建立使用者：

```bash
azure vm reset-access -g exampleResourceGroup -n exampleVMName -u userexample -p examplePassword
```

## 移除使用者
```bash
azure vm reset-access -g exampleResourceGroup -n exampleVMName -R userexample
```

## 重設 SSHD
重設 SSHD 組態：

```bash
azure vm reset-access -g exampleResourceGroup -n exampleVMName -r
```


## 詳細的逐步解說
### VMAccess 已定義︰
Linux VM 的磁碟顯示錯誤。您不知怎麼重設 Linux VM的根密碼，或不小心刪除了 SSH 私密金鑰。如果是過去資料中心的時代發生此狀況，您必須親赴現場，然後再開啟 KVM 才能存取伺服器主控台。請將 Azure VMAccess 擴充功能想成 KVM 交換器，在此可以存取主控台重設 Linux 存取或執行磁碟等級維護。

在詳細的逐步解說中，我們會使用利用原始 JSON 檔案的完整格式 VMAccess。從 Azure 範本也可以呼叫這些 VMAccess JSON 檔案。

### 使用 VMAccess 檢查或修復 Linux VM 的磁碟
使用 VMAccess 可以執行在 Linux VM 磁碟上執行的 fsck。您也可以使用 VMAccess 執行磁碟檢查和磁碟修復。

若要檢查並修復磁碟，請使用這個 VMAccess 指令碼︰

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

執行 VMAccess 指令碼搭配︰

```bash
azure vm extension set exampleResourceGroup exampleVM \
VMAccessForLinux Microsoft.OSTCExtensions * \
--private-config-path disk_check_repair.json
```

### 使用 VMAccess 重設 Linux 的使用者存取
如已無法存取 Linux VM 的根，您可以啟動 VMAccess 指令碼來重設根密碼。

若要重設根密碼，請使用這個 VMAccess 指令碼︰

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"exampleNewPassword",   
}
```

執行 VMAccess 指令碼搭配︰

```bash
azure vm extension set exampleResourceGroup exampleVM \
VMAccessForLinux Microsoft.OSTCExtensions * \
--private-config-path reset_root_password.json
```

若要重設非根使用者的 SSH 金鑰，請使用這個 VMAccess 指令碼︰

`reset_ssh_key.json`

```json
{
  "username":"exampleUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== exampleUser@exampleServer",   
}
```

執行 VMAccess 指令碼搭配︰

```bash
azure vm extension set exampleResourceGroup exampleVM \
VMAccessForLinux Microsoft.OSTCExtensions * \
--private-config-path reset_ssh_key.json
```

### 使用 VMAccess 管理 Linux 的使用者帳戶
VMAccess 是一種 Python 指令碼，可用來管理 Linux VM 上的使用者，但不需要登入及使用 sudo 或根帳戶。

若要建立使用者，請使用這個 VMAccess 指令碼︰

`create_new_user.json`

```json
{
"username":"exampleNewUserName",
"ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== exampleUser@exampleServer",
"password":"examplePassword",
}
```

執行 VMAccess 指令碼搭配︰

```bash
azure vm extension set exampleResourceGroup exampleVM \
VMAccessForLinux Microsoft.OSTCExtensions * \
--private-config-path create_new_user.json
```

若要建立使用者：

`remove_user.json`

```json
{
"remove_user":"exampleUser",
}
```

執行 VMAccess 指令碼搭配︰

```bash
azure vm extension set exampleResourceGroup exampleVM \
VMAccessForLinux Microsoft.OSTCExtensions * \
--private-config-path remove_user.json
```

### 使用 VMAccess 重設 SSHD 組態
如果您變更了 Linux VM SSHD 組態，並在驗證變更之前關閉 SSH 連線，您可能無法回到 SSH 作業。VMAccess 可用來將 SSHD 組態重設回到已知的良好組態，而不需透過 SSH 登入。

使用這個 VMAccess 指令碼重設 SSHD 組態：

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

執行 VMAccess 指令碼搭配︰

```bash
azure vm extension set exampleResourceGroup exampleVM \
VMAccessForLinux Microsoft.OSTCExtensions * \
--private-config-path reset_sshd.json
```

## 後續步驟
使用 Azure VMAccess 延伸模組更新 Linux 是變更執行中的 Linux VM 的一種方法。您也可以使用類似 cloud-init 和 Azure 範本等工具，以在開機時修改您的 Linux VM。

[有關虛擬機器擴充功能和功能](virtual-machines-linux-extensions-features.md)

[使用 Linux VM 擴充功能編寫 Azure Resource Manager 範本](virtual-machines-linux-extensions-authoring-templates.md)

[在建立期間使用 cloud-init 自訂 Linux VM](virtual-machines-linux-using-cloud-init.md)

<!---HONumber=AcomDC_0831_2016-->