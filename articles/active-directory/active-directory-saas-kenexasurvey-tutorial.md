---
title: 教學課程：Azure Active Directory 與 IBM Kenexa Survey Enterprise 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 IBM Kenexa Survey Enterprise 之間的單一登入。
services: active-directory
documentationcenter: ''
author: jeevansd
manager: femila
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2016
ms.author: jeedes

---
# 教學課程：Azure Active Directory 與 IBM Kenexa Survey Enterprise 整合
在本教學課程中，您會了解如何整合 IBM Kenexa Survey Enterprise 與 Azure Active Directory (Azure AD)。

IBM Kenexa Survey Enterprise 與 Azure AD 整合提供下列優點：

* 您可以在 Azure AD 中控制可存取 IBM Kenexa Survey Enterprise 的人員
* 您可以讓使用者使用他們的 Azure AD 帳戶自動登入 IBM Kenexa Survey Enterprise (單一登入)
* 您可以在 Azure 傳統入口網站中集中管理您的帳戶

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](active-directory-appssoaccess-whatis.md)。

## 必要條件
若要設定 Azure AD 與 IBM Kenexa Survey Enterprise 整合，您需要下列項目：

* Azure AD 訂用帳戶
* 已啟用 IBM Kenexa Survey Enterprise 單一登入的訂用帳戶

> [!NOTE]
> 若要測試本教學課程中的步驟，我們不建議使用生產環境。
> 
> 

若要測試本教學課程中的步驟，您應該遵循這些建議：

* 除非必要，否則您不應使用生產環境，。
* 如果您沒有 Azure AD 試用環境，您可以在[這裡](https://azure.microsoft.com/pricing/free-trial/)取得一個月試用。

## 案例描述
在本教學課程中，您會在測試環境中測試 Azure AD 單一登入。本教學課程中說明的案例由二個主要建置組塊組成：

1. 從資源庫新增 IBM Kenexa Survey Enterprise
2. 設定並測試 Azure AD 單一登入

## 從資源庫新增 IBM Kenexa Survey Enterprise
若要設定將 IBM Kenexa Survey Enterprise 整合到 Azure AD 中，您需要從資源庫將 IBM Kenexa Survey Enterprise 新增到受管理的 SaaS 應用程式清單。

**若要從資源庫新增 IBM Kenexa Survey Enterprise，請執行下列步驟：**

1. 在 **Azure 傳統入口網站**中，按一下左方瀏覽窗格的 [Active Directory]。
   
    ![Active Directory][1]
2. 從 [目錄] 清單中，選取要啟用目錄整合的目錄。
3. 若要開啟應用程式檢視，請在目錄檢視中，按一下頂端功能表中的 [應用程式]。
   
    ![應用程式][2]
4. 按一下頁面底部的 [新增]。
   
    ![應用程式][3]
5. 在 [欲執行動作] 對話方塊中，按一下 [從資源庫中新增應用程式]。
   
    ![應用程式][4]
6. 在搜尋方塊中輸入 **IBM Kenexa Survey Enterprise**。
   
    ![建立 Azure AD 測試使用者](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_01.png)
7. 在結果窗格中選取 [IBM Kenexa Survey Enterprise]，然後按一下 [完成] 以新增應用程式。
   
    ![建立 Azure AD 測試使用者](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_02.png)

## 設定並測試 Azure AD 單一登入
在本節中，您會以名為 "Britta Simon" 的測試使用者身分，使用 IBM Kenexa Survey Enterprise 設定及測試 Azure AD 單一登入。

若要讓單一登入運作，Azure AD 必須知道 IBM Kenexa Survey Enterprise 與 Azure AD 中互相對應的使用者。換句話說，必須建立 Azure AD 使用者和 IBM Kenexa Survey Enterprise 中相關使用者之間的連結關聯性。

建立此連結關聯性的方法是將 Azure AD 中**使用者名稱**的值指派為 IBM Kenexa Survey Enterprise 中 **Username** 的值。

若要設定及測試與 IBM Kenexa Survey Enterprise 搭配運作的 Azure AD 單一登入，您需要完成下列構成要素：

1. **[設定 Azure AD 單一登入](#configuring-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[建立 Azure AD 測試使用者](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
3. **[建立 IBM Kenexa Survey Enterprise 測試使用者](#creating-an-kenexasurvey-test-user)** - 在 IBM Kenexa Survey Enterprise 中建立 Britta Simon 的對應項目，且該項目與 Azure AD 中代表 Britta Simon 的項目連結。
4. **[指派 Azure AD 測試使用者](#assigning-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[測試單一登入](#testing-single-sign-on)** - 驗證組態是否能運作。

### 設定 Azure AD 單一登入
在本節中，您會在傳統入口網站中啟用 Azure AD 單一登入，並在您的 IBM Kenexa Survey Enterprise 中設定單一登入。

**若要使用 IBM Kenexa Survey Enterprise 設定 Azure AD 單一登入，請執行下列步驟：**

1. 在傳統入口網站的 [IBM Kenexa Survey Enterprise ] 應用程式整合頁面上，按一下 [設定單一登入] 來開啟 [設定單一登入] 對話方塊。
   
    ![設定單一登入][6]
2. 在 [要如何讓使用者登入 IBM Kenexa Survey Enterprise ] 頁面上，選取 [Azure AD 單一登入]，然後按 [下一步]。
   
    ![設定單一登入](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_03.png)
3. 在 [設定 App 設定] 對話方塊頁面執行下列步驟：
   
    ![設定單一登入](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_04.png)

    a.在 [登入 URL] 文字方塊中，輸入使用者用登入 IBM Kenexa Survey Enterprise 應用程式的 URL，輸入格式為：**“https://surveys.kenexa.com/<company code>”**。

    b.在 [識別碼] 文字方塊中輸入下列格式的 URL：**https://surveys.kenexa.com/\<CompanyCode>/tools**。

    > [AZURE.NOTE] 請提供識別碼 URL 給 IBM Kenexa Survey 團隊，並請他們將此資料設為您的執行個體的實體識別碼。


    c.按 [下一步]。


1. 在 [在 IBM Kenexa Survey Enterprise 設定單一登入] 頁面上，執行下列步驟︰
   
    ![設定單一登入](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_05.png)
   
    a.按一下 [下載憑證]，然後將檔案儲存在您的電腦上。
   
    b.按 [下一步]。
2. 若要為您的應用程式設定 SSO，請連絡 IBM Kenexa 支援小組，並提供下列資訊：
   
   * 下載的憑證檔案
   * **簽發者 URL**
   * **SAML SSO URL**
   * **單一登出服務 URL**
     
     > [!NOTE]
     > 請注意，回應中的 NameID 宣告值必須符合 Kenexa 系統中設定的 SSO 識別碼。因此請與 Kenexa 支援小組合作，以將您組織中適當的使用者識別碼對應為 SSO 識別碼。Azure AD 預設會將 NameIdentifier 設為 UPN 值。您可以如以下螢幕擷取畫面所示，在 [屬性] 索引標籤中變更此值。在完成正確對應後，整合才有作用。
     > 
     > 
     
     ![設定單一登入](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_51.png)
3. 在傳統入口網站中，選取單一登入設定確認項目，然後按一下 [下一步]。
   
    ![Azure AD 單一登入][10]
4. 在 [單一登入確認] 頁面上，按一下 [完成]。
   
    ![Azure AD 單一登入][11]

### 建立 Azure AD 測試使用者
在本節中，您會在傳統入口網站中建立名稱為 Britta Simon 的測試使用者。

![建立 Azure AD 使用者][20]

**若要在 Azure AD 中建立測試使用者，請執行下列步驟：**

1. 在 **Azure 傳統入口網站**中，按一下左方瀏覽窗格的 [Active Directory]。
   
    ![建立 Azure AD 測試使用者](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_09.png)
2. 從 [目錄] 清單中，選取要啟用目錄整合的目錄。
3. 若要顯示使用者清單，請按一下頂端功能表的 [使用者]。
   
    ![建立 Azure AD 測試使用者](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_03.png)
4. 若要開啟 [加入使用者] 對話方塊，請按一下底部工具列上的 [加入使用者]。
   
    ![建立 Azure AD 測試使用者](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_04.png)
5. 在 [告訴我們這位使用者] 對話方塊頁面上，執行以下步驟：
   
    ![建立 Azure AD 測試使用者](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_05.png)
   
    a.針對 [使用者類型]，選取 [您組織中的新使用者]。
   
    b.在 [使用者名稱] 文字方塊中，輸入 **BrittaSimon**。
   
    c.按 [下一步]。
6. 在 [使用者設定檔]對話方塊頁面上，執行下列步驟：
   
   ![建立 Azure AD 測試使用者](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_06.png)
   
   a.在 [名字] 文字方塊中，輸入 **Britta**。
   
   b.在 [姓氏] 文字方塊中，輸入 **Simon**。
   
   c.在 [顯示名稱] 文字方塊中，輸入 **Britta Simon**。
   
   d.在 [角色] 清單中選取 [使用者]。
   
   e.按 [下一步]。
7. 在 [取得暫時密碼] 對話方塊頁面上，按一下 [建立]。
   
    ![建立 Azure AD 測試使用者](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_07.png)
8. 在 [取得暫時密碼] 對話方塊頁面上，執行下列步驟：
   
    ![建立 Azure AD 測試使用者](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_08.png)
   
    a.記下 [新密碼] 的值。
   
    b.按一下 [完成]。

### 建立 IBM Kenexa Survey Enterprise 測試使用者
在本節中，您要在 IBM Kenexa Survey Enterprise 中建立名為 Britta Simon 的使用者。請與 IBM Kenexa 支援小組合作，以對應所有使用者的 SSO 識別碼。此 SSO 識別碼值也應該從 Azure AD 對應至 NameIdentifier 值。您可以在 [屬性] 索引標籤中變更此預設設定。

> [!NOTE]
> 如果您需要手動建立使用者，您需要連絡 IBM Kenexa Survey Enterprise 支援小組。
> 
> 

### 指派 Azure AD 測試使用者
在本節中，您會把 IBM Kenexa Survey Enterprise 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

![指派使用者][200]

**若要將 Britta Simon 指派給 IBM Kenexa Survey Enterprise，請執行下列步驟：**

1. 在傳統入口網站中，若要開啟應用程式檢視，請在目錄檢視中，按一下頂端功能表中的 [應用程式]。
   
    ![指派使用者][201]
2. 在應用程式清單中，選取 [IBM Kenexa Survey Enterprise]。
   
    ![設定單一登入](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_50.png)
3. 在頂端的功能表中，按一下 [使用者]。
   
    ![指派使用者][203]
4. 在 [使用者] 清單中，選取 [Britta Simon]。
5. 在底部的工具列中，按一下 [指派]。
   
    ![指派使用者][205]

### 測試單一登入
在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在「存取面板」中按一下 [IBM Kenexa Survey Enterprise] 圖格時，應該會自動登入您的 IBM Kenexa Survey Enterprise 應用程式。

## 其他資源
* [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](active-directory-saas-tutorial-list.md)
* [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0713_2016-->