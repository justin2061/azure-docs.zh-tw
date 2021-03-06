---
title: 使用 Azure 入口網站為 Azure SQL Database 設定異地複寫 | Microsoft Docs
description: 使用 Azure 入口網站為 Azure SQL Database 設定異地複寫
services: sql-database
documentationcenter: ''
author: stevestein
manager: jhubbard
editor: ''

ms.service: sql-database
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/14/2016
ms.author: sstein

---
# 使用 Azure 入口網站為 Azure SQL Database 設定異地複寫
> [!div class="op_single_selector"]
> * [概觀](sql-database-geo-replication-overview.md)
> * [Azure 入口網站](sql-database-geo-replication-portal.md)
> * [PowerShell](sql-database-geo-replication-powershell.md)
> * [T-SQL](sql-database-geo-replication-transact-sql.md)
> 
> 

本文說明如何使用 [Azure 入口網站](http://portal.azure.com)為 SQL Database 設定「主動式異地複寫」。

若要使用 Azure 入口網站起始容錯移轉，請參閱[使用 Azure 入口網站為 Azure SQL Database 起始計劃性或非計劃性容錯移轉](sql-database-geo-replication-failover-portal.md)。

> [!NOTE]
> 主動式異地複寫 (可讀取次要複本) 現在可供所有服務層中的所有資料庫使用。在 2017 年 4 月，不可讀取的次要類型將淘汰，而現有不可讀取的資料庫將自動升級為可讀取的次要複本。
> 
> 

若要使用 Azure 入口網站設定異地複寫，您需要下列項目：

* Azure 訂用帳戶。
* Azure SQL Database 資料庫 - 您想要複寫到不同地理區域的主要資料庫。

## 加入次要資料庫
下列步驟會在「異地複寫」合作關係中建立新的次要資料庫。

若要加入次要資料庫，您必須是訂閱擁有者或共同擁有者。

次要資料庫的名稱會與主要資料庫相同，並且預設會具有相同的服務層級。次要資料庫可以是可讀取或不可讀取，並且可以是單一資料庫或彈性資料庫。如需詳細資訊，請參閱[服務層](sql-database-service-tiers.md)。
建立並植入次要複本之後，就會開始從主要資料庫將資料複寫到新的次要資料庫。

> [!NOTE]
> 如果夥伴資料庫已經存在 (例如，因終止先前的「異地複寫」關聯性所導致)，命令將會失敗。
> 
> 

### 加入次要
1. 在 [Azure 入口網站](http://portal.azure.com)中，瀏覽至您想要為「異地複寫」設定的資料庫。
2. 在 [SQL Database] 刀鋒視窗上，選取 [所有設定] > [異地複寫]。
3. 選取要建立次要資料庫的區域。

    ![加入次要][1]


1. 設定 [次要類型] \([可讀取] 或 [不可讀取])。
2. 選取或設定次要資料庫的伺服器。
   
    ![建立次要][3]
3. (選擇性) 您可以將次要資料庫加入彈性資料庫集區中：
   
       - 按一下 [彈性資料庫集區]，然後選取目標伺服器上要在其中建立次要資料庫的集區。集區必須已經存在於目標伺服器上，因為此工作流程並不會建立新集區。
4. 按一下 [建立] 以加入次要資料庫。
5. 將會建立次要資料庫並開始植入程序。
   
    ![植入][6]
6. 當植入程序完成時，次要資料庫會顯示其狀態 (不可讀取)。
   
    ![次要就緒][9]

## 移除次要資料庫
此作業會永久終止對次要資料庫的複寫，並將次要資料庫的角色變更為一般讀寫資料庫。如果與次要資料庫的連線中斷，命令將會成功，但次要資料庫必須等到連線恢復後才會變成讀寫資料庫。

1. 在 [Azure 入口網站](http://portal.azure.com)中，瀏覽至「異地複寫」合作關係中的主要資料庫。
2. 在 [SQL Database] 刀鋒視窗上，選取 [所有設定] > [異地複寫]。
3. 在 [次要] 清單中，選取您想要從「異地複寫」合作關係中移除的資料庫。
4. 按一下 [**停止複寫**]。
   
    ![移除次要][7]
5. 按一下 [停止複寫] 會開啟一個確認視窗，請按一下 [是] 以將資料庫從「異地複寫」合作關係中移除 (將它設定為不屬於任何複寫的讀寫資料庫)。

    ![確認移除][8]


## 後續步驟
* 若要深入了解主動式異地複寫，請參閱[主動式異地複寫](sql-database-geo-replication-overview.md)
* 如需商務持續性概觀和案例，請參閱[商務持續性概觀](sql-database-business-continuity.md)

<!--Image references-->
[1]: ./media/sql-database-geo-replication-portal/configure-geo-replication.png
[2]: ./media/sql-database-geo-replication-portal/add-secondary.png
[3]: ./media/sql-database-geo-replication-portal/create-secondary.png
[4]: ./media/sql-database-geo-replication-portal/secondary-type.png
[5]: ./media/sql-database-geo-replication-portal/create.png
[6]: ./media/sql-database-geo-replication-portal/seeding0.png
[7]: ./media/sql-database-geo-replication-portal/remove-secondary.png
[8]: ./media/sql-database-geo-replication-portal/stop-confirm.png
[9]: ./media/sql-database-geo-replication-portal/seeding-complete.png
[10]: ./media/sql-database-geo-replication-portal/failover.png

<!---HONumber=AcomDC_0727_2016-->