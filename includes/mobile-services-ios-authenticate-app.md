* 開啟 **QSTodoListViewController.m**，並加入下列方法：如果您不使用 Facebook 作為識別提供者，請將 *facebook* 變更為 *microsoftaccount*、*twitter*、*google* 或 *windowsazureactivedirectory*。

```
        - (void) loginAndGetData
        {
            MSClient *client = self.todoService.client;
            if (client.currentUser != nil) {
                return;
            }

            [client loginWithProvider:@"facebook" controller:self animated:YES completion:^(MSUser *user, NSError *error) {
                [self refresh];
            }];
        }
```

* 使用下列程式碼取代 `viewDidLoad` 中的 `[self refresh]`：

```
        [self loginAndGetData];
```

* 按下 [**執行**] 以啟動應用程式，然後登入。當您登入時，應該能夠檢視待辦事項清單並進行更新。

<!---HONumber=Oct15_HO3-->