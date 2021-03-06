1. 按一下 [Azure 管理入口網站](https://manage.windowsazure.com/)之目錄頁面上的 [應用程式] 索引標籤。
2. 按一下整合的應用程式登錄。
3. 按一下應用程式頁面上的 [設定]，並向下捲動至頁面中的 [金鑰] 區段。
4. 針對新金鑰的期限，按一下 [1 年]。然後按一下 [儲存]，入口網站將會顯示您的新金鑰值。
5. 複製儲存後顯示的**用戶端識別碼**和**金鑰**。請注意，儲存後金鑰值只會顯示一次。 
   
    ![](./media/mobile-services-generate-aad-app-registration-access-key/client-id-and-key.png)
6. 向下捲動整合應用程式設定頁面至底部，並啟用應用程式的 [讀取目錄資料] 權限，然後按一下 [儲存]。
   
    ![](./media/mobile-services-generate-aad-app-registration-access-key/app-perms.png)
7. 在 [Azure 傳統入口網站](https://manage.windowsazure.com/)中，往回瀏覽至您的行動服務，並按一下 [設定] 索引標籤。向下捲動至 [應用程式設定] 區段，並加入下列應用程式設定，然後按一下 [儲存]。
   
    <table border="1"> <tr> <th>應用程式設定名稱</th><th>說明</th> </tr> <tr> <td>AAD\_CLIENT\_ID</td><td>在上述步驟中，您從整合應用程式複製的用戶端識別碼。</td> </tr> <tr> <td>AAD\_CLIENT\_KEY</td><td>在上述步驟中，AAD 整合應用程式中產生的應用程式金鑰。</td> </tr> <tr> <td>AAD\_TENANT\_DOMAIN</td><td>您的 AAD 網域名稱。應類似於 "mydomain.onmicrosoft.com"</td> </tr> </table><br/>

    ![](./media/mobile-services-generate-aad-app-registration-access-key/aad-app-settings.png)


<!---HONumber=AcomDC_1203_2015-->