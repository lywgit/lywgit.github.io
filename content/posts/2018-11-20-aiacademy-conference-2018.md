+++
date = '2018-11-20T00:00:00+08:00'
title = '2018 台灣人工智慧學校校友年會'
tags = ['event', 'ml', 'zh']
+++


| 活動 |     |
|------|-----|
| 名稱 | 2018 台灣人工智慧學校校友年會 （[議程](https://aiacademy.tw/conference2018/)） |
| 日期 | 2018.11.17 (六) |
| 地點 | 中央研究院 台北南港 |


## 演講隨記

### *簡立峰* / Google Taiwan 董事總經理

* 對社會大眾來說，**「自駕車上路的成功或失敗」將會是下一個對 AI 最顯著的印象**。
    * waymo 計程車美國上路
* 台灣：工程師之島
    * 內部小，所以先出去再成功，或留在台灣 in house 軟體代工，目前軟體人力資源是缺乏的。
* leverage 大公司 API，不一定要自己做
    * 都想要自己做的人可能會錯過時機（速度）。
    * 台灣強項在於軟硬上快速 prototype 能力。 
* 台灣其實資源也是很集中在大公司的，只是屬於 B2B 所以沒有影響百業。
    * 韓國大企業 B2C。B2C 會有直接 data iteration （得到直接的 user feedback）
    * 沒有的要考慮放掉 
* 原生網路公司以外的企業很多尚沒有 data-driven culture，沒有好的 data pipeline
* **AI 並不一定要能做到像人一樣的理解才會「有用」**
    * 人類（醫生）無法很快讀五百篇文章、也很難記得所有內容（即使理解更正確），因此會有幫得上忙的地方。
        * 電腦有記憶和速度的強項，AI 不是要讓電腦完全像人一樣
    * Chatbot 現在無法做很好的脈絡回應（例如怎樣的天氣算「好」其實非常取決於脈絡），但即使只是簡單、不精確的回答晴天雨天也有所幫助。


### *林彥宇* / 中央研究院資訊科技創新研究中心副研究員

* low frame rate -> high frame rate vedio image interpolation
    * 低 framerate 可以節省，但使用者感覺不好
* 傳統方法
    * 單純取先後 frame 平均並不好。
    * Optical flow
* CNN
    * predict optical flow: flownet
    * 直接 predict intermediat frame DVFlow Liu ICCV17 
        * 缺點: 容易有 artifact 或模糊（oversmooth），因為 L1 L2 loss 對 smooth 圖容忍大
* 提出 cycle consistency checking
    * cycle loss
    * cycle + motion linearity loss(假設短時間linear motion) 
    * cycle + Edge guided training（解決邊緣模糊）
    * full （含以上）  
* benchmark dataset: UCF-101, Middleburry



### *王淳恆* / 沐恩生醫光電股份有限公司技術長

* 人工智慧與醫療應用
* 競賽區別是 iphone 還是 galaxy 拍的相片模型正確率可以高達 97%！但是模型學習到的分辨方式是基於影像的雜訊：不同品牌相機自己的 pipeline 導致最後雜訊的不同。
    * 好：機器辨識可利用到人看不出來的資訊
    * 壞：如果希望模型學會的是某些已知訊號的話，我們就必須擔心到底學會的是什麼。
* **模型不可解釋的話，在醫療上有用嗎？**
* 可解釋性 AI
    * Grad-CAM [arXiv 1610.02391](https://arxiv.org/abs/1610.02391). 看狗臉、貓尾
* SHAP value: SHapley Additive exPlanation: Shapley value 
    * 讓我們知道模型是不是真的在看我們看的 signal
    * [GitHub 連結](https://github.com/slundberg/shap), [arXiv 1705.07874](https://arxiv.org/pdf/1705.07874.pdf)
        * 應用：data 不同型態都可以，但模型不一定： xgboost CNN 可以 LSTM 目前不行 
* Explainable AI is importat in Medial AI 

* Q&A： 需要多少訓練樣本？
    * 眼睛就差不多看得出來的話，約 1000 筆就可以做到還可以。
    * 肉眼不太容易或是要更好的話，可能要上萬筆了。 


### *徐宏民 Winston* / 國立臺灣大學資訊工程學系暨研究所教授

* Productizing (or Monetizing) Neural Networks Technologies 產品化深度學習核心技術
* Watson 剪預告片 AI movie trailers
    * 電影公司雖然請你做這件事，但也不會願意就給你所有的 data，不是把他所有 movie + trailer 拿來訓練就好
* 很多創意來自員工，不是老闆
* AI: augmented intellegence
* 人臉辨識問題：低解析度、面具、墨鏡、化妝
    * superresolution, face hallucination and recognition
    * 加上 super-identity loss, super-pixel loss
* VR 看鞋子在人身上 Chou et al ACCV 18 （將發表）
* 空拍機即時計算車輛：[Drone-based Object Counting by Spatially Regularized Regional Proposal Network](https://arxiv.org/abs/1707.05972)
* 一些經驗
    * **Impact 來自顧客需求**
    * 產品發展和研究不可分
* How to get quality data in an effective and efficient way
    * data crawing
    * (weakly) semi-supervised
    * data augmentation
        * *幫人臉資料戴眼鏡後再 train 幫助很大*。
    * GAN (但必須跨domain其他knowledge)


### *施敬修 (James Shih)* / 英特爾台灣分公司物聯網平台應用總監

* AI 邊緣運算 - Intel 跨平台的視覺運算解決方案
* Edge computing is fundamental for IoT. 
* **三個階段**
    1. connected (IoT)
    2. smart (Big data)
    3. autonomous (AI)
* Intel 轉定位自己在資料運算
* by 2021, 82% IP traffic will be vedio
* OpenVINO：跨framework開發工具，同樣開發一討軟體可放在所有 intel CPU 平台上面
    * edge 端可以有不同強度的硬體 offer: cpu gpu fpga   
* ecosystem 下接硬體廠商，上接雲端



### *李明達* / 開源機器人俱樂部創辦人

* 推薦 SSD512 for 瑕疵檢測：找小瑕疵好
* 醫療常用 mask-CNN
* 只要目前作業員透過螢幕判斷的瑕疵檢測，AI都可以做得更好
* 產線上 caffe inference 較快，醫療影像 pytorch多
* AI 是個賺錢工具


### *高智敏* / 勤業眾信聯合會計師事務所經理

* AI 所面臨之風險與可能因應之道
* 世界經濟論壇：高風險、高機會
    * 偏見歧視：用歐美資料訓練的照相普著軟體誤判亞洲人在眨眼；法官輔助系統判斷黑人再犯率高（但這個判斷錯誤率很高）
        * 可解釋 AI：**讓人更理解 AI 的決策，也助於注意到偏見**
            * 讓自駕車顯示「我在等你過馬路」
        * IBM data fairness 360：data 是否有偏見
    * 偷模型：透過使用來反推（竊取）模型參數
    * 侵犯隱私：透過滑手機習慣可以查出情緒


### *趙質忠 (Jason Tsao)* / 微軟大中華區人工智慧暨數位轉型負責人

* **AI 只是工具，重點是你要做什麼**
    * 小心為了 AI 而 AI
* Microsoft 定位自己是平台、工具：不進去跟客戶競爭、不用客戶的數據做生意（例如不去看你的 email，有寫在合約裡面）
* 微軟研究院：人與電腦聽說讀寫
* 各種功能以 container 形式提供，可以線下跑


### *施晨揚* / PIXNET Algorithm Engineering Manager

* Ex1 分類照片：食物、場景、菜單
    * 資料：google 搜尋圖片下載（菜單），用python 下載之類的 等於讓 google 先挑第一輪了，只是再人工 verify（借力）
* Ex2 圖片 embedding 影像搜尋
    * 用 xception encode 圖片，忽略小數字（以節省空間）
* Ex3 未登入使用者是誰？
    * 行為 -> 性別、年齡
    * Naive bayse：好跟人解釋是一個優點
* Ex4 content ranking
    * 過濾不良文章，人工是來不及的
    * CNN
    
    
### *張寅建* / IBM 雲端事業部雲端資深顧問

* IA: Information Archetecture
* 沒有 IA 哪來 AI


## 思考

* 個人容易受限於自己想像中的世界。多聽演講，了解別人關注的問題，想想為什麼。
* 現實問題是重要動機，多注意人的需求，避免為了做而做。
* AI 和人的關係
    * 不是互斥或完全取代性的
    * AI 也並不是要達到人的水準才會有用。
    * 讓自駕車對行人顯示「等你過馬路」，甚至裝上會看著行人的眼睛，會增加信任。
    * Keep human in the loop
* 執著於做到完美其實往往不是最完美的作法。
    * 現實中速度與正確性都有重要之處。想像 Loss function 同時包含正確性與時效項。
    * 把借力納入可能。
* 想想知識能創造什麼價值（價值未必是錢）
    


