---
title: "Machine Learning Studio 中的簡易實驗 | Microsoft Docs"
description: "此機器學習服務教學課程會引導您輕鬆進行資料科學實驗。 我們將使用迴歸演算法預測汽車價格。"
keywords: "實驗、線性迴歸、機器學習服務演算法、機器學習服務教學課程、預測性模型技術、資料科學實驗"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b6176bb2-3bb6-4ebf-84d1-3598ee6e01c6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/14/2016
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 210e19cbc581ce5fc17898abe184b96c48370e7f


---
# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a>機器學習服務教學課程：在 Azure Machine Learning Studio 中建立您的第一個資料科學實驗
此機器學習服務教學課程會引導您輕鬆進行資料科學實驗。 我們將建立一個線性迴歸模型，該模型會根據使用製造和技術規格等不同變數，來預測汽車的價格。 為了這樣做，我們將使用 Azure Machine Learning Studio 來開發及檢測簡易的預測性分析實驗。

*預測性分析* 是一種使用目前的資料來預測未來結果的資料科學 。 如需預測性分析的簡單範例，請看適用於初學者的資料科學影片 4： [以簡易模型預測答案](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) (執行階段︰7:42)。

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a>Machine Learning Studio 如何協助您？
Machine Learning Studio 可讓您輕鬆地使用以預測性模型技術預先程式化的拖放模組來設定實驗。 若要執行實驗並預測答案，您可以使用 Machine Learning Studio 以「建立模型」、「訓練模型」和「對模型評分與測試」。

進入 Machine Learning Studio： [https://studio.azureml.net](https://studio.azureml.net)。 如果您之前已登入過 Machine Learning Studio，按一下 **由此登入**。 否則按一下 **註冊** 並選擇免費或付費選項。

如需 Machine Learning Studio 的一般資訊，請參閱 [什麼是 Machine Learning Studio？](machine-learning-what-is-ml-studio.md)

## <a name="five-steps-to-create-an-experiment"></a>建立實驗的五個步驟
在這個機器學習教學課程中，您可以遵循下列五個基本步驟，在 Machine Learning Studio 中建置實驗，來建立、訓練及評分您的模型：

* 建立模型
  * [步驟 1：取得資料]
  * [步驟 2：前置處理資料]
  * [步驟 3：定義功能]
* 訓練模型
  * [步驟 4：選擇及套用學習演算法]
* 對模型評分與測試
  * [步驟 5：預測新的汽車價格]

[步驟 1：取得資料]: #step-1-get-data
[步驟 2：前置處理資料]: #step-2-preprocess-data
[步驟 3：定義功能]: #step-3-define-features
[步驟 4：選擇及套用學習演算法]: #step-4-choose-and-apply-a-learning-algorithm
[步驟 5：預測新的汽車價格]: #step-5-predict-new-automobile-prices


## <a name="step-1-get-data"></a>步驟 1：取得資料
Machine Learning Studio 隨附多個範例資料集供您選擇，而且您可以從許多來源匯入資料。 在此範例中，我們將使用內含的範例資料集 **汽車價格資料 (原始)**。
此資料集將包含許多個別汽車的項目，包括製造、模型、技術規格和價格等資訊。

1. 按一下 Machine Learning Studio 視窗底部的 [+新增]，並選取 [實驗]，然後選取 [空白實驗] 以開始新的實驗。 選取畫布頂端的預設實驗名稱，然後將它重新命名為有意義的名稱，例如，**汽車價格預測**。
2. 實驗畫布左側是資料集和模組的調色盤。 在此調色盤頂端的 [搜尋] 方塊中輸入**汽車**，可尋找標示為**汽車價格資料 (原始)** 的資料集。
   
    ![Palette search][screen1a]
3. 將資料集拖曳到實驗畫布。
   
    ![Dataset][screen1]

若想知道此資料的呈現情形，請按一下汽車資料集底部的輸出連接埠，然後選取 [視覺化] 。

![模組的輸出連接埠][screen1c]

資料集中的變數會顯示為資料行，汽車的各個執行個體會顯示為資料列。 最右側的資料行 (資料行 26，其標題為「價格」) 是我們要嘗試預測的目標變數。

![Dataset visualization][screen1b]

按一下右上角的 "**x**"，以關閉視覺化視窗。

## <a name="step-2-preprocess-data"></a>步驟 2：前置處理資料
資料集通常必須先經過某些前置處理，才能進行分析。 您可能已經注意到在各種不同資料列的資料行中有遺漏的值。 必須清除這些遺漏的值，讓模型才能正確地分析資料。 在我們的案例中，我們將移除含有遺漏值的所有資料列。 此外， **自負虧損** 資料行含有比例很高的遺漏值，因此我們會將該資料行從模型中完全排除。

> [!TIP]
> 在使用大部分的模組時，都必須從輸入資料中清除遺漏值。
> 
> 

首先我們會移除 **自負虧損** 資料行，接著我們會移除任何含有遺漏資料的資料列。

1. 在模組調色盤頂端的 [搜尋] 方塊中輸入**選取資料行**以尋找[選取資料集中的資料行][select-columns] 模組，然後將其拖曳到實驗畫布，並連接至**汽車價格資料 (原始)** 資料集的輸出連接埠。 此模組可讓我們選取要將哪些資料行包含在模型中，或是從模型中排除。
2. 選取[選取資料集中的資料行][select-columns] 模組，然後按一下 [屬性] 窗格中的 [啟動資料行選取器]。
   
   * 在左側按一下 [套用規則] 
   * 在 [開始於] 下，按一下 [所有資料行]。 這會引導[選取資料集中的資料行][select-columns] 通過所有資料行 (但即將排除的資料行除外)。
   * 在下拉式清單中，選取 [排除] 和 [資料行名稱]，然後按一下文字方塊內部。 資料行清單隨即顯示。 選取**自負虧損**，該資料行就會新增到文字方塊中。
   * 按一下核取記號 ([確定]) 按鈕，以關閉資料行選取器。
     
     ![Select columns][screen3]
     
     [選取資料集中的資料行] 的屬性窗格指出它會通過資料集中的所有資料行，但**自負虧損**除外。
     
     ![選取資料集中的資料行屬性][screen4]
     
     > [!TIP]
     > 您可以按兩下模組並輸入文字，為模組新增註解。 這有助於您快速檢視模組在您實驗中的執行情況。 在此案例中，按兩下[選取資料集中的資料行][select-columns] 模組，然後輸入註解「排除自負虧損」。
     > 
     > 
3. 將[清除遺漏的資料][clean-missing-data] 模組拖曳到實驗畫布，然後將其連接到[選取資料集中的資料行][select-columns] 模組。 在 [屬性] 窗格中，在 [清除模式] 下，選取 [移除整個資料列]，藉由移除含有遺漏值的資料列來清除資料。 按兩下模組，並輸入註解「移除遺漏值資料列」。
   
    ![清除遺漏資料屬性][screen4a]
4. 按一下實驗畫布下方的 [ **執行** ]，以執行實驗。

實驗完成時，所有模組都會呈現綠色核取標記，表示它們已順利完成。 同時也請留意位於右上角的 **執行完成** 狀態。

![First experiment run][screen5]

我們的實驗到目前為止所完成的事項為清除資料。 若要檢視已清除的資料集，請按一下[清除遺漏資料][clean-missing-data] 模組 (「清理的資料集」) 左邊的輸出連接埠，然後選取 [視覺化]。 請注意，此時已不包含 **自負虧損** 資料行，而且也沒有遺漏值。

現在我們已清除資料，而可以指定要在預測模型中使用哪些功能。

## <a name="step-3-define-features"></a>步驟 3：定義功能
在機器學習中， *功能* 是您感興趣的某項目的個別可測量的屬性。 在我們的資料集中，每個資料列都代表一部汽車，而每個資料行是該汽車的功能。

要找到一組理想的功能來建立預測模型，必須針對您要解決的問題進行實驗，並具備相關知識。 有些功能會比其他功能更適合用來預測目標。 此外，有些功能會與其他功能高度關聯 (例如 city-mpg 與 highway-mpg)，因此將不會在模型中新增太多新資訊，而且可以移除這些功能。

我們將建置一個模型，並在其中使用我們資料集中的功能子集。 您可以重新選取不同功能、再次執行實驗，然後確認是否會有較理想的結果。 但是若要開始，讓我們試用下列功能︰

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. 將另一個[選取資料集中的資料行][select-columns] 模組拖曳到實驗畫布，並將它連接至[清除遺漏資料][clean-missing-data] 模組左側的輸出連接埠。 按兩下模組，並輸入「選取要預測的功能」。
2. 按一下 [屬性] 窗格中的 [啟動資料行選取器]。
3. 按一下 [套用規則] 。
4. 在 [開始於] 下，按一下 [無資料行]，然後在篩選資料列中選取 [包含] 和 [資料行名稱]。 輸入資料行名稱的清單。 這會引導模組僅通過我們指定的資料行。
   
   > [!TIP]
   > 藉由執行實驗，我們確定資料的資料行定義已從資料集通過[清除遺漏的資料][clean-missing-data] 模組。 這表示您連接的其他模組也會具有資料集的資訊。
   > 
   > 
5. 按一下核取記號 ([確定]) 按鈕。

![Select columns][screen6]

這會產生將在後續步驟的學習演算法中使用的資料集。 稍後您可以返回，並選取不同的功能重新嘗試。

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a>步驟 4：選擇及套用學習演算法
現在，資料已備妥，訓練和測試是建構預測模型的要素。 我們將使用我們的資料來訓練模型，然後測試模型，查看它如何精準預測價格。 現在不必擔心為什麼需要訓練然後測試模型。

「分類」和「迴歸」是兩種受監督的機器學習技術。 「分類」可從一組已定義的類別預測答案，例如色彩 (紅色、藍色或綠色)。 「迴歸」可用來預測數字。

因為要預測價格，也就是一個數字，因此我們將使用迴歸模型。 在此範例中，我們將訓練簡易 *線性迴歸* 模型，並在下一個步驟中加以測試。

1. 我們將資料分割成個別的訓練集和測試集，用來進行訓練和測試。 選取[分割資料][split] 模組並拖曳到實驗畫布，然後將其連接到最後一個[選取資料集中的資料行][select-columns] 模組的輸出。 將 [Fraction of rows in the first output dataset]  設為 0.75。 如此，我們將使用百分之 75 的資料來訓練模型，並保留百分之 25 做為測試之用。
   
   > [!TIP]
   > 變更 [ **隨機種子** ] 參數，可讓您為訓練和測試產生不同的隨機範例。 此參數會控制虛擬隨機數字產生器的植入。
   > 
   > 
2. 執行實驗。 這可讓[選取資料集中的資料行][select-columns] 和[分割資料][split] 模組將資料行定義傳遞至我們接下來要新增的模組。  
3. 若要選取學習演算法，請在畫布左側的模組調色盤中展開 [機器學習服務] 類別，然後展開 [初始化模型]。 這會顯示數個可用來初始化機器學習演算法的模組類別。
   
    在此實驗中，請選取 [迴歸] 類別下的[線性迴歸][linear-regression] 模組 (您也可以在調色盤搜尋方塊中輸入「線性迴歸」以尋找模組)，然後將其拖曳到實驗畫布。
4. 找出[訓練模型][train-model] 模組，並將其拖曳到實驗畫布。 將左側的輸入連接埠連接到[線性迴歸][linear-regression] 模組的輸出。 將右側的輸入連接埠連接到[分割資料][split] 模組的訓練資料輸出 (左側連接埠)。
5. 選取[訓練模型][train-model] 模組，按一下 [屬性] 窗格中的 [啟動資料行選取器]，然後選取 [價格] 資料行。 這是我們的模型所將預測的值。
   
    ![選取「價格」資料行][screen7]
6. 執行實驗。

結果是經過訓練的迴歸模型，可用來為新範例評分以進行預測。

![套用機器學習演算法][screen8]

## <a name="step-5-predict-new-automobile-prices"></a>步驟 5：預測新的汽車價格
現在已完成使用百分之 75 資料模型的訓練，我們可以用它來為其他百分之 25 的資料評分，以了解模型的運作是否理想。

1. 尋找[評分模型][score-model] 模組，並將其拖曳至實驗畫布，然後將左側的輸入連接埠連接到[訓練模型][train-model] 模組的輸出。 將右側的輸入連接埠連接到[分割資料][split] 模組的測試資料輸出 (右側連接埠)。  
   
    ![Score Model module][screen8a]
2. 若要執行實驗，並檢視[評分模型][score-model] 模組的輸出，按一下輸出連接埠，然後選取 [視覺化]。 輸出會顯示價格的預測值，以及來自測試資料的已知值。  
3. 最後，若要測試結果的品質，選取[評估模型][evaluate-model] 模組，並將其拖曳至實驗畫布，然後將左側的輸入連接埠連接到[評分模型][score-model] 模組的輸出 (輸入連接埠有兩個是因為[評估模型][evaluate-model] 模組可以用來比較兩個模型)。
4. 執行實驗。

若要檢視[評估模型][evaluate-model] 模組的輸出 (按一下輸出連接埠，然後選取 [視覺化])。 我們的模型會顯示下列統計資料：

* **平均絕對誤差** (MAE)：絕對誤差的平均值 ( *誤差* 是指預測值與實際值之間的差異)。
* **均方根誤差** (RMSE)：對測試資料集所做之預測的平方誤差的評分根平均值。
* **相對絕對誤差**：相對於實際值與所有實際值之平均值之間的絕對差異的絕對誤差平均值。
* **相對平方誤差**：相對於實際值與所有實際值之平均值之間的平方差異的平方誤差平均值。
* **決定係數**：也稱為 **R 平方值**，這是一個統計度量，可指出模型對於資料的適用程度。

針對每個誤差統計資料，越小越好。 值越小，表示預測越接近實際值。 就 [決定係數] 而言，其值愈接近一 (1.0)，預測就愈精準。

![Evaluation results][screen9]

實驗最終應呈現如下：

![機器學習教學課程：完成使用預測性模型化技術的線性迴歸實驗。][screen10]

## <a name="next-steps"></a>後續步驟
現在您已經完成第一個機器學習教學課程，並已設定實驗，您就能逐一檢測以嘗試改善模型。 例如，您可以變更您在預測中使用的功能。 或者，您可以修改[線性迴歸][linear-regression] 演算法的屬性，或嘗試完全不同的演算法。 您甚至可以同時在實驗中新增多個機器學習演算法，並使用[評估模型][evaluate-model] 模組進行比較 (兩兩相比)。

> [!TIP]
> 使用實驗畫布下方的 [ **另存新檔** ] 按鈕，可複製實驗的任何檢測。 您可以按一下畫布下方的 [ **檢視執行歷程記錄** ]，以檢視實驗的所有檢測。 如需詳細資訊，請參閱[在 Azure Machine Learning Studio 中管理實驗檢測][runhistory]。
> 
> 

[runhistory]: machine-learning-manage-experiment-iterations.md

如果您對模型感到滿意，您可以將其部署為 Web 服務，用以透過新資料預測汽車價格。 如需詳細資訊，請參閱[部署 Azure Machine Learning Web 服務][publish]。

[publish]: machine-learning-publish-a-machine-learning-web-service.md

如需適用於建立、訓練、評分及部署模型之預測性模型化技術的更廣泛且更詳細的逐步解說，請參閱[使用 Azure Machine Learning 開發預測性解決方案][walkthrough]。

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[screen1]:./media/machine-learning-create-experiment/screen1.png
[screen1a]:./media/machine-learning-create-experiment/screen1a.png
[screen1b]:./media/machine-learning-create-experiment/screen1b.png
[screen1c]: ./media/machine-learning-create-experiment/screen1c.png
[screen2]:./media/machine-learning-create-experiment/screen2.png
[screen3]:./media/machine-learning-create-experiment/screen3.png
[screen4]:./media/machine-learning-create-experiment/screen4.png
[screen4a]:./media/machine-learning-create-experiment/screen4a.png
[screen5]:./media/machine-learning-create-experiment/screen5.png
[screen6]:./media/machine-learning-create-experiment/screen6.png
[screen7]:./media/machine-learning-create-experiment/screen7.png
[screen8]:./media/machine-learning-create-experiment/screen8.png
[screen8a]:./media/machine-learning-create-experiment/screen8a.png
[screen9]:./media/machine-learning-create-experiment/screen9.png
[screen10]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/



<!--HONumber=Nov16_HO2-->


