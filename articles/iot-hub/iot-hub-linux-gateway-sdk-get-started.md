---
title: "開始使用 IoT 中樞閘道 SDK | Microsoft Docs"
description: "此 Azure IoT 閘道 SDK 逐步解說使用 Linux 來說明您使用 Azure IoT 閘道 SDK 時應該了解的重要概念。"
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: cf537bdd-2352-4bb1-96cd-a283fcd3d6cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2016
ms.author: andbuc
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 23176a9251a90a985a5d2fbce23ceeb9d0925234


---
# <a name="azure-iot-gateway-sdk-beta-get-started-using-linux"></a>Azure IoT 閘道 SDK (Beta) - 開始使用 Linux
[!INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>如何建置範例
開始之前，您必須[設定開發環境][lnk-setupdevbox]以在 Linux 上使用 SDK。

1. 開啟殼層。
2. 瀏覽至 **azure-iot-gateway-sdk** 儲存機制本機複本中的根資料夾。
3. 執行 **tools/build.sh** 指令碼。 此指令碼使用 **cmake** 公用程式，在 **azure-iot-gateway-sdk** 存放庫本機複本的根資料夾中建立稱為 **build** 的資料夾，以及產生 Makefile。 此指令碼接著會建置方案，並執行測試。

> [!NOTE]
> 每次執行 **build.sh** 指令碼時，都會先在 **azure-iot-gateway-sdk** 存放庫本機複本的根資料夾中刪除再重新建立 **build** 資料夾。
> 
> 

## <a name="how-to-run-the-sample"></a>如何執行範例
1. **build.sh** 指令碼會在 **azure-iot-gateway-sdk** 儲存機制本機複本的 **build** 資料夾中產生其輸出。 這包含在此範例中使用的兩個模組。
   
    build 指令碼會將 **liblogger_hl.so** 放在 **build/modules/logger/** 資料夾中，並將 **libhello_world_hl.so** 放在 **build/modules/hello_world/** 資料夾中。 使用這些路徑作為「模組路徑」  值 (如下面的 JSON 設定檔案中所示)。
2. **samples/hello_world/src** 資料夾中的 **hello_world_lin.json** 檔案是適用於 Linux 且可用來執行範例的範例 JSON 設定檔案。 下面顯示的範例 JSON 設定假設您將從 **azure-iot-gateway-sdk** 儲存機制本機複本的根資料夾執行範例。
3. 針對 **logger_hl** 模組，在 **args** 區段中，將 **filename** 值設定為包含記錄資料的檔案名稱和路徑。
   
    這是適用於 Linux 且將 **log.txt** 寫入您執行範例之資料夾的 JSON 設定檔案範例。
   
    ```
    {
      "modules" :
      [ 
        {
          "module name" : "logger_hl",
          "loading args": {
            "module path" : "./build/modules/logger/liblogger_hl.so"
          },
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "loading args": {
            "module path" : "./build/modules/hello_world/libhello_world_hl.so"
          },
          "args" : null
        }
      ],
      "links" :
      [
        {
          "source": "hello_world",
          "sink": "logger_hl"
        }
      ]
    }
    ```
4. 在您的殼層中，瀏覽至 **azure-iot-gateway-sdk** 資料夾。
5. 執行以下命令：
   
   ```
   ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
   ``` 

[!INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md



<!--HONumber=Nov16_HO2-->


