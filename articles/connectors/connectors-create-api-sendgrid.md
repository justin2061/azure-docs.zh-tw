---
title: SendGrid | Microsoft Docs
description: 使用 Azure App Service 建立邏輯應用程式。SendGrid 連線提供者可讓您傳送電子郵件及管理收件者清單。
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: ''
tags: connectors

ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: deonhe

---
# 開始使用 SendGrid 連接器
SendGrid 連線提供者可讓您傳送電子郵件及管理收件者清單。

> [!NOTE]
> 這一版的文章適用於邏輯應用程式 2015-08-01-preview 結構描述版本。
> 
> 

您可以從建立邏輯應用程式立即開始，請參閱[建立邏輯應用程式](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## 觸發程序及動作
SendGrid 連接器可當做動作使用，它有觸發程序。所有連接器都支援 JSON 和 XML 格式的資料。

 SendGrid 連接器提供下列動作。但不包含觸發程序。

### SendGrid 動作
您可以採取下列動作：

| 動作 | 說明 |
| --- | --- |
| [SendEmail](connectors-create-api-sendgrid.md#sendemail) |使用 SendGrid API 傳送電子郵件 (限制為 10,000 名收件者) |
| [AddRecipientToList](connectors-create-api-sendgrid.md#addrecipienttolist) |將個別的收件者加入收件者清單中 |

## 建立 SendGrid 的連線
若要使用 SendGrid 建立邏輯應用程式，您必須先建立**連接**，然後提供下列屬性的詳細資料︰

| 屬性 | 必要 | 說明 |
| --- | --- | --- |
| ApiKey |是 |提供您的 SendGrid API 金鑰 |

> [!INCLUDE [建立至 SendGrid 連線的步驟](../../includes/connectors-create-api-sendgrid.md)]
> 
> [!TIP]
> 您可以在其他邏輯應用程式中使用這個連接。
> 
> 

建立連線後，您就可以用它執行動作，並接聽本文所述的觸發程序。

## SendGrid 參考
適用的版本：1.0

## SendEmail
傳送電子郵件：使用 SendGrid API 傳送電子郵件 (限制為 10,000 名收件者)

```POST: /api/mail.send.json```

| Name | 資料類型 | 必要 | 位於 | 預設值 | 說明 |
| --- | --- | --- | --- | --- | --- |
| 要求 | |yes |body |無 |要傳送的電子郵件訊息 |

#### Response
| Name | 說明 |
| --- | --- |
| 200 |OK |
| 400 |不正確的要求 |
| 401 |未經授權 |
| 403 |禁止 |
| 404 |找不到 |
| 429 |要求太多 |
| 500 |內部伺服器錯誤。發生未知錯誤 |
| 預設值 |作業失敗。 |

## AddRecipientToList
將收件者加入清單中：將個別的收件者加入收件者清單中

```POST: /v3/contactdb/lists/{listId}/recipients/{recipientId}```

| 名稱 | 資料類型 | 必要 | 位於 | 預設值 | 說明 |
| --- | --- | --- | --- | --- | --- |
| 清單識別碼 |string |yes |路徑 |無 |收件者清單的唯一識別碼 |
| recipientId |string |yes |路徑 |無 |收件者的唯一識別碼 |

#### Response
| 名稱 | 說明 |
| --- | --- |
| 200 |OK |
| 400 |不正確的要求 |
| 401 |未經授權 |
| 403 |禁止 |
| 404 |找不到 |
| 500 |內部伺服器錯誤。發生未知錯誤 |
| 預設值 |作業失敗。 |

## 物件定義
### EmailRequest
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| from |string |是 |
| fromname |string |否 |
| to |string |是 |
| toname |string |否 |
| 主旨 |string |是 |
| body |string |是 |
| ishtml |布林值 |否 |
| 副本 |string |否 |
| ccname |string |否 |
| 密件副本 |string |否 |
| bccname |string |否 |
| replyto |string |否 |
| 日期 |string |否 |
| headers |string |否 |
| 檔案 |array |否 |
| 檔名 |array |否 |

### EmailResponse
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| 訊息 |string |否 |

### RecipientLists
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| 清單 |array |否 |

### RecipientList
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| id |integer |否 |
| 名稱 |string |否 |
| recipient\_count |integer |否 |

### 收件者
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| 收件者 |array |否 |

### 收件者
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| 電子郵件 |string |否 |
| last\_name |string |否 |
| first\_name |string |否 |
| id |string |否 |

## 後續步驟
[建立邏輯應用程式](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!---HONumber=AcomDC_0824_2016-->