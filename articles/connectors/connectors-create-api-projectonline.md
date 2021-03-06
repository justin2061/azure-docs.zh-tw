---
title: ProjectOnline | Microsoft Docs
description: 使用 Azure App Service 建立邏輯應用程式。Project Online 是專案組合管理 (PPM) 與 Microsoft 日常工作的彈性線上方案。Project Online 透過 Office 365 傳遞，可讓組織快速開始使用功能強大的專案管理功能，來規劃、設定優先順序以及管理專案和專案組合投資，不受場地和裝置的限制。
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
# 開始使用 ProjectOnline 連接器
Project Online 是專案組合管理 (PPM) 與 Microsoft 日常工作的彈性線上方案。Project Online 透過 Office 365 傳遞，可讓組織快速開始使用功能強大的專案管理功能，來規劃、設定優先順序以及管理專案和專案組合投資，不受場地和裝置的限制。

> [!NOTE]
> 這一版的文章適用於邏輯應用程式 2015-08-01-preview 結構描述版本。
> 
> 

您可以從建立邏輯應用程式立即開始，請參閱[建立邏輯應用程式](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## 觸發程序及動作
ProjectOnline 連接器可當成動作使用，它有觸發程序。所有連接器都支援 JSON 和 XML 格式的資料。

 ProjectOnline 連接器提供下列動作及/或觸發程序：

### ProjectOnline 動作
您可以採取下列動作：

| 動作 | 說明 |
| --- | --- |
| [ListProjects](connectors-create-api-projectonline.md#listprojects) |列出 Project Online 網站中的專案 |
| [CreateProject](connectors-create-api-projectonline.md#createproject) |在 Project Online 網站中建立新專案 |
| [CreateTask](connectors-create-api-projectonline.md#createtask) |在專案中建立新工作 |
| [CreateResource](connectors-create-api-projectonline.md#createresource) |在 Project Online 網站中建立企業資源 |
| [ListTasks](connectors-create-api-projectonline.md#listtasks) |列出專案中已發佈的工作 |
| [CheckoutProject](connectors-create-api-projectonline.md#checkoutproject) |簽出網站中的專案 |
| [PublishProject](connectors-create-api-projectonline.md#publishproject) |簽入和發佈網站中現有的專案 |

### ProjectOnline 觸發程序
您可以接聽下列事件：

| 觸發程序 | 說明 |
| --- | --- |
| 建立新專案時 |每當建立新專案就會觸發流程 |
| 建立新資源時 |建立新資源時就會觸發新流程 |
| 建立新工作時 |建立新工作時就會觸發流程 |

## 建立 ProjectOnline 的連線
若要使用 ProjectOnline 建立邏輯應用程式，您必須先建立**連接**，然後提供下列屬性的詳細資料︰

| 屬性 | 必要 | 說明 |
| --- | --- | --- |
| 權杖 |是 |提供 ProjectOnline 認證 |

> [!INCLUDE [建立至 ProjectOnline 連線的步驟](../../includes/connectors-create-api-projectonline.md)]
> 
> [!TIP]
> 您可以在其他邏輯應用程式中使用這個連接。
> 
> 

## ProjectOnline 的參考
適用的版本：1.0

## OnNewProject
建立新專案時：每當建立新專案就會觸發流程

```GET: /trigger/_api/ProjectData/Projects```

| 名稱 | 資料類型 | 必要 | 位於 | 預設值 | 說明 |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |yes |query |無 |專案網站的根網站 URL (範例︰https://sampletenant.sharepoint.com/teams/sampleteam) |

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

## OnNewResource
建立新資源時︰建立新資源時就會觸發新流程

```GET: /trigger/_api/ProjectData/Resources```

| 名稱 | 資料類型 | 必要 | 位於 | 預設值 | 說明 |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |yes |query |無 |專案網站的根網站 URL (範例︰https://sampletenant.sharepoint.com/teams/sampleteam) |

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

## OnNewTask
建立新工作時︰建立新工作時就會觸發流程

```GET: /trigger/_api/ProjectData/Tasks```

| 名稱 | 資料類型 | 必要 | 位於 | 預設值 | 說明 |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |yes |query |無 |專案網站的根網站 URL (範例︰https://sampletenant.sharepoint.com/teams/sampleteam) |

#### Response
| Name | 說明 |
| --- | --- |
| 200 |OK |
| 400 |不正確的要求 |
| 401 |未經授權 |
| 403 |禁止 |
| 404 |找不到 |
| 500 |內部伺服器錯誤。發生未知錯誤 |
| 預設值 |作業失敗。 |

## ListProjects
列出專案：列出 Project Online 網站中的專案

```GET: /_api/ProjectServer/Projects```

| 名稱 | 資料類型 | 必要 | 位於 | 預設值 | 說明 |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |yes |query |無 |專案網站的根網站 URL (範例︰https://sampletenant.sharepoint.com/teams/sampleteam) |

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

## CreateProject
建立新專案：在 Project Online 網站中建立新專案

```POST: /_api/ProjectServer/Projects```

| Name | 資料類型 | 必要 | 位於 | 預設值 | 說明 |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |yes |query |無 |專案網站的根網站 URL (範例︰https://sampletenant.sharepoint.com/teams/sampleteam) |
| proj | |yes |body |無 |要建立的新專案 |

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

## CreateTask
建立新工作：在專案中建立新工作

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Tasks/Add```

| 名稱 | 資料類型 | 必要 | 位於 | 預設值 | 說明 |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |yes |query |無 |專案網站的根網站 URL (範例︰https://sampletenant.sharepoint.com/teams/sampleteam) |
| project\_id |string |yes |路徑 |無 |要加入工作中的專案唯一識別碼 |
| 工作 | |yes |body |無 |要加入專案中的新工作 |

#### Response
| Name | 說明 |
| --- | --- |
| 200 |OK |
| 400 |不正確的要求 |
| 401 |未經授權 |
| 403 |禁止 |
| 404 |找不到 |
| 500 |內部伺服器錯誤。發生未知錯誤 |
| 預設值 |作業失敗。 |

## CreateResource
建立新資源：在 Project Online 網站中建立企業資源

```POST: /_api/ProjectServer/EnterpriseResources```

| 名稱 | 資料類型 | 必要 | 位於 | 預設值 | 說明 |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |yes |query |無 |專案網站的根網站 URL (範例︰https://sampletenant.sharepoint.com/teams/sampleteam) |
| 資源 | |yes |body |無 |要加入專案中的新企業資源 |

#### Response
| Name | 說明 |
| --- | --- |
| 200 |OK |
| 400 |不正確的要求 |
| 401 |未經授權 |
| 403 |禁止 |
| 404 |找不到 |
| 500 |內部伺服器錯誤。發生未知錯誤 |
| 預設值 |作業失敗。 |

## ListTasks
列出工作：列出專案中已發佈的工作

```GET: /_api/ProjectServer/Projects('{project_id}')/Tasks```

| Name | 資料類型 | 必要 | 位於 | 預設值 | 說明 |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |yes |query |無 |專案網站的根網站 URL (範例︰https://sampletenant.sharepoint.com/teams/sampleteam) |
| project\_id |string |yes |路徑 |無 |擷取工作的專案唯一識別碼 |

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

## CheckoutProject
簽出專案：簽出網站中的專案

```POST: /_api/ProjectServer/Projects('{project_id}')/checkOut```

| Name | 資料類型 | 必要 | 位於 | 預設值 | 說明 |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |yes |query |無 |專案網站的根網站 URL (範例︰https://sampletenant.sharepoint.com/teams/sampleteam) |
| project\_id |string |yes |路徑 |無 |要加入工作中的專案唯一識別碼 |

#### Response
| Name | 說明 |
| --- | --- |
| 200 |OK |
| 400 |不正確的要求 |
| 401 |未經授權 |
| 403 |禁止 |
| 404 |找不到 |
| 500 |內部伺服器錯誤。發生未知錯誤 |
| 預設值 |作業失敗。 |

## PublishProject
簽入和發佈專案：簽入和發佈網站中現有的專案

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Publish(true)```

| 名稱 | 資料類型 | 必要 | 位於 | 預設值 | 說明 |
| --- | --- | --- | --- | --- | --- |
| siteUrl |string |yes |query |無 |專案網站的根網站 URL (範例︰https://sampletenant.sharepoint.com/teams/sampleteam) |
| project\_id |string |yes |路徑 |無 |簽入的專案唯一識別碼 |

#### Response
| Name | 說明 |
| --- | --- |
| 200 |OK |
| 400 |不正確的要求 |
| 401 |未經授權 |
| 403 |禁止 |
| 404 |找不到 |
| 500 |內部伺服器錯誤。發生未知錯誤 |
| 預設值 |作業失敗。 |

## 物件定義
### TriggerProjectsWrapper
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| value |array |否 |

### TriggerProject
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| ProjectStartDate |string |否 |
| ProjectFinishDate |string |否 |
| ProjectCreatedDate |string |否 |
| ProjectId |string |否 |
| ProjectModifiedDate |string |否 |
| ProjectType |integer |否 |
| ProjectName |string |否 |

### TriggerResourcesWrapper
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| value |array |否 |

### TriggerResource
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| ResourceId |string |否 |
| ResourceBaseCalendar |string |否 |
| ResourceBookingType |integer |否 |
| ResourceCanLevel |布林值 |否 |
| ResourceCostPerUse |number |否 |
| ResourceCreatedDate |string |否 |
| ResourceEarliestAvailableFrom |string |否 |
| ResourceEmail |string |否 |
| ResourceInitials |string |否 |
| ResourceIsActive |布林值 |否 |
| ResourceIsGeneric |布林值 |否 |
| ResourceLatestAvailableTo |string |否 |
| ResourceModifiedDate |string |否 |
| ResourceName |string |否 |
| ResourceStatsuName |string |否 |
| ResourceType |integer |否 |
| TypeDescription |string |否 |
| TypeName |string |否 |

### TriggerTasksWrapper
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| value |array |否 |

### TriggerTask
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| ProjectId |string |否 |
| TaskId |string |否 |
| ProjectName |string |否 |
| TaskName |string |否 |
| TaskCreatedDate |string |否 |
| TaskModifieddate |string |否 |
| TaskStartDate |string |否 |
| TaskFinishDate |string |否 |
| TaskPriority |integer |否 |
| TaskIsActive |布林值 |否 |

### NewProject
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| 名稱 |string |是 |
| 說明 |string |否 |
| 啟動 |string |否 |

### NewReource
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| 名稱 |string |是 |
| IsBudget |布林值 |否 |
| IsGeneric |布林值 |否 |
| IsInactive |布林值 |否 |

### 隨附此逐步解說的專案
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| ApprovedStart |string |否 |
| ApprovedEnd |string |否 |
| CheckedOutDate |string |否 |
| CheckOutDescription |string |否 |
| CheckOutId |string |否 |
| CreatedDate |string |否 |
| 識別碼 |string |否 |
| IsCheckedOut |布林值 |否 |
| LastPublishedDate |string |否 |
| LastSavedDate |string |否 |
| OptimizerDecision |integer |否 |
| PlannerDecision |integer |否 |
| ProjectType |integer |否 |
| 名稱 |string |否 |
| WinprojVersion |string |否 |

### ProjectsWrapper
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| value |array |否 |

### NewTask
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| 參數 |沒有定義 |是 |

### TaskParameters
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| 名稱 |string |是 |
| 注意事項 |string |否 |
| 啟動 |string |否 |
| 持續時間 |string |否 |

### EnterpriseResource
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| CanLevel |布林值 |否 |
| 代碼 |string |否 |
| CostAccrual |integer |否 |
| CostCenter |string |否 |
| 建立時間 |string |否 |
| DefaultBookingType |integer |否 |
| 電子郵件 |string |否 |
| ExternalId |string |否 |
| 群組 |string |否 |
| HireDate |string |否 |
| 識別碼 |string |否 |
| Initials |string |否 |
| IsActive |布林值 |否 |
| IsBudget |布林值 |否 |
| IsCheckedOut |布林值 |否 |
| IsGeneric |布林值 |否 |
| IsTeam |布林值 |否 |
| MaterialLabel |string |否 |
| 修改時間 |string |否 |
| 名稱 |string |否 |
| Phonetics |string |否 |
| ResourceType |integer |否 |
| TerminationDate |string |否 |

### TasksWrapper
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| value |array |否 |

### 工作
| 屬性名稱 | 資料類型 | 必要 |
| --- | --- | --- |
| 建立時間 |string |否 |
| 修改時間 |string |否 |
| 啟動 |string |否 |
| 完成 |string |否 |
| Name |string |否 |
| 識別碼 |string |否 |
| 優先順序 |integer |否 |
| PercentComplete |integer |否 |
| 注意事項 |string |否 |
| 連絡人 |string |否 |

## 後續步驟
[建立邏輯應用程式](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!---HONumber=AcomDC_0824_2016-->