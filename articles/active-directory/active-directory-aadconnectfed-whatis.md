---
title: Azure AD Connect 和同盟 | Microsoft Docs
description: 此頁面是有關使用 Azure AD Connect 執行 AD FS 作業的所有文件的中心位置
services: active-directory
documentationcenter: ''
author: anandyadavmsft
manager: femila
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2016
ms.author: anandy

---
# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect 和同盟
Azure AD Connect 可讓您設定與內部部署 AD FS 和 Azure AD 同盟。 使用同盟登入，您可以讓使用者使用內部部署密碼登入 Azure AD 服務；使用公司網路時，無須再次輸入密碼就可登入服務。 使用 AD FS 的同盟選項可讓您在 Windows Server 2012 R2 伺服器陣列中部署新的或指定現有的 AD FS。

本主題是 Azure AD Connect 的「同盟」相關功能的主要資訊來源，並列出其他所有相關主題的連結。 如需 Azure AD Connect 的連結，請參閱＜整合內部部署身分識別與 Azure Active Directory＞。

## <a name="azure-ad-connect---federation-topics"></a>Azure AD Connect - 同盟主題
| 主題 | 涵蓋內容和讀取時機 |
|:--- |:--- |
| **Azure AD Connect 使用者登入選項** | |
| [了解使用者登入選項](active-directory-aadconnect-user-signin.md) |描述各種使用者登入選項及其如何影響 Azure 登入使用者體驗 |
| **使用 Azure AD Connect 安裝 AD FS** | |
| [必要條件](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) |透過 Azure AD Connect 成功安裝 AD FS 安裝的必要條件 |
| [設定 AD FS 伺服器陣列](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) |如何使用 Azure AD Connect 安裝新的 AD FS 伺服器陣列 |
| **修改 AD FS 組態** | |
| [修復信任](active-directory-aadconnect-federation-management.md#reparing-the-trust) |如何修復內部部署 AD FS 和 Office 365 / Azure 之間目前的信任 |
| [新增新的 AD FS 伺服器](active-directory-aadconnect-federation-management.md#adding-a-new-ad-fs-server) |初始安裝後增加 AD FS 伺服器來擴充 AD FS 伺服器陣列 |
| [新增新的 AD FS WAP 伺服器](active-directory-aadconnect-federation-management.md#adding-a-new-wap-server) |初始安裝後增加 WAP 伺服器來擴充 AD FS 伺服器陣列 |
| [新增新的同盟網域](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain) |新增要與 Azure AD 同盟的其他網域 |
| **後續安裝工作** | |
| [新增自訂公司標誌/圖例](active-directory-aadconnect-federation-management.md#add-custom-company-logo-or-illustration) |指定將顯示在 AD FS 登入頁面的自訂標誌來修改登入體驗 |
| [新增登入說明](active-directory-aadconnect-federation-management.md#add-sign-in-description) |變更 AD FS 登入頁面上的登入說明 |
| [修改 AD FS 宣告規則](active-directory-aadconnect-federation-management.md#modifying-ad-fs-claim-rules) |修改/新增 AD FS 中對應至 Azure AD Connect 同步組態的宣告規則 |

## <a name="additional-resources"></a>其他資源
* [整合內部部署身分識別與 Azure Active Directory](active-directory-aadconnect.md)
* [Azure 中的 AD FS 部署](active-directory-aadconnect-azure-adfs.md)

<!--HONumber=Oct16_HO2-->


