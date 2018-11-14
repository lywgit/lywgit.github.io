---
title: Curiosity-driven Exploration by Self-supervised Prediction 
category: paper-note
tag: [reinforcement-learning, curiosity, self-supervised-learning, zh]
---

> 環境獎勵稀少的強化學習問題往往會面臨學習很沒有效率的困難，其中一種改善方法是設定內部獎勵機制來引導 agent 多多探索未知的部分，進而提高成功的機會。文章提出 Intrinsic Curiosity Module（**ICM**）架構用以計算內部獎勵。ICM 包含兩部分：（1）Forward dynamics model：學習根據目前狀態和採取的動作預測新的 featurized state，以及（2） Inverse dynamics model：學習看先後兩個 featurized state 反推 agent 曾採取的 action，藉此以自監督式學習的方式訓練出 raw state 中與 agent 的行為真正相關的表徵。使用 featurized space 的預測誤差作為內部獎勵（而不是 raw state 的預測誤差）可以避免隨機或與 agent 無關的資訊干擾以好奇心獎勵 agent 多探索的設計。


| 文章 |     |
|-----|-----|
| 標題 | Curiosity-driven Exploration by Self-supervised Prediction  |  
| 作者 | Deepak Pathak, Pulkit Agrawal, Alexei A. Efros, Trevor Darrell (2017) |
| 連結 | [arXiv 1705.05363](https://arxiv.org/abs/1705.05363) / [blog](https://pathak22.github.io/noreward-rl/)| 


## 主要思路

#### 一、問題

* 在強化學習（Reinforcement learning RL）問題中，agent 與環境互動並得到來自環境的獎勵（reward），目標是讓 agent 從與環境的互動經驗學習到一套能成功取得獎勵的策略 policy。不過，很多情況下外部獎勵是稀少的，例如迷宮探索問題可能只在到達終點時才會有獎勵，如果需要很長一系列正確的操作才可能達到終點時，靠著隨機探索可能連成功一次都很困難，導致幾乎沒有獎勵訊號供有效的學習。
* 改善這問題的方法之一種是設計內部獎勵訊號（intrinsic reward signal）來鼓勵 agent 多探索未知的可能（而不只是隨機選取），以提高成功的機會。具體作法之一是讓 agent 用一個 environment dynamics 的模型來學習預測 \\(s_t, a_t \rightarrow s_{t+1}\\)），再以「預測錯誤」的程度作為內部獎勵，誘使 agent 往未知的方向探索。
* 但這種設計會面臨一種困難：agent 觀察到的 raw state （例如遊戲畫面的 pixel values）往往帶有隨機成分或是與 action 根本無關的變化，這會導致 agent 再怎麼探索都無法有效的改善對新 state 的預測（誤差一直很大），致使 agent 困在這裡，而破壞了「獎勵探索」的目的。 

#### 二、目標

* 設計一種能避開上述困難的好奇心獎勵機制。

#### 三、方法

* 提出 **Intrinsic Curiosity Module （ICM）** 的架構以計算 curiosity-driven intrinsic reward。提取 raw state \\(s_t\\) 中受 agent 動作影響的特徵 \\(\phi(s_t)\\) 並忽略隨機不相關的變化。對此特徵的預測誤差（比起對 raw state 的預測誤差）更適合作為好奇心內部獎勵。
* Agent 包含兩部分：
    1. Policy：依然透過 policy gradient 方式學習策略（例如 A3C 方法）。
    2. Reward Generator：ICM 以 \\((s_t, a_t, s_{t+1})\\) 為輸入，輸出內部獎勵 \\(r^i_t\\)。
* ICM 架構又分成兩部分：
    1. *Forward dynamics model*：根據舊的 state 與 action 預測新的 featurized state \\(\hat{\phi}(s_{t+1})\\)。訓練時最小化預測的 loss \\(L_F(\hat{\phi}(s_{t+1}), \phi(s_{t+1})) \\)。
        * 使用對 \\(\phi(s)\\) 預測的錯誤大小當內部獎勵 \\(r^i_t\\)
    2. *Inverse dynamics model*：根據先後兩個連續的 featurized state \\(\phi(s_t), \phi(s_{t+1})\\) 反預測 agent 採取的動作 \\(\hat{a}_t\\)。訓練時最小化預測的 loss \\( L_I(\hat{a}_t, a_t) \\) 。 
        * Inverse dynamics model 的訓練包含找出好的 featurization \\(\phi\\)。

     
#### 四、結果

* 從三個方向看「好奇心」扮演的角色：
    1. 稀少的外部獎勵下，「好奇心」可以利用較少的互動次數更快的學會達成目標。
    2. 無外部獎勵的情況下「好奇心」可以驅使更有效率的探索。
    3. 泛化能力：僅用好奇心（無外部獎勵）的 pre-train 亦有助於新的關卡時的探索。
* *VisDoom*, *Super Mario Bros*

#### 五、貢獻

* 提出一種 curiosity-driven intrinsic reward 的計算機制，能學習自 raw state 中取出與本身 action 相關部分的 representation。改善環境不可預測的雜訊干擾 intrinsic reward 功能的問題。
* Agent 可以在完全「沒有外部獎勵」的情況下靠著好奇心的指引更有效率的探索迷宮。

## 其他概念

<small>
註：\\(s_t\\) 為 (raw) state，而 \\(\phi(s)\\) 應可理解為 state 的 feature、encoding、embedding 或是 representation。我尚未掌握這些詞的區別。 
</small>

#### 「好奇心」內部獎勵機制與困難

* 為了改善強化學習問題中問題裡 reward 稀少、學習不容易的問題而設計的內部獎勵有兩種常見作法：
    1. 鼓勵 agent 探索新的 state。Agent 需要建立「環境的 state 分佈」的模型。 
    2. 鼓勵 agent 探索自己「預測行為後果」錯誤較高的行動。Agent 需要建立「自己行為帶來後果」的模型，即 environmental dynamics：\\((s_t, a_t)\rightarrow s_{t+1} \\)。
* 本文的方法屬第二類，至於為何這樣的設定有類似好奇心的效果可以這樣想：
    * 預測行動後果的錯誤越高 \\(\rightarrow\\) 之前沒什麼探索這一塊。因為 dynamics model 一直都在學習做更出好的預測，如果已經經過多次探索，則錯誤應該已被改善。
    * 因此，用「預測行動後果的錯誤」來當內部獎勵時便相當於鼓勵 agent 多探索掌握較差的部分。因為 agent 選取下一步動作時通常以最大化（內部加外部）獎勵為方向。
* 困難：
    1. Agent 的 environmental dynamics 模型一般難以處理高維度、連續的 state。例如以遊戲畫面 raw pixel values 為 state 時，要正確預測這樣的 state space 本身就有難度，所以本文改預測 featurized state 會比較容易（個人理解）。 
    2. Agent-environment system 可能具有隨機性：有可能是操縱 agent 時的不確定性、抑或是環境本身就有隨機變化的因素。而這會影響好奇心獎勵設計的功效。
        * 想像一個 agent 發現自己對（有雜訊的）電視畫面影像預測錯誤偏高，於是鼓勵自己執行某動作並希望能降低 prediction error。但改善對 random noise 的預測是不可能的，這會使得「預測越差」無法反映「探索較少」這件事。


#### 為何透過訓練 Inverse dynamics model 能找到適合計算好奇心獎勵的 feature？

1. 從一個狀態到下一個狀態 \\(s_{t}, a_t\rightarrow s_{t+1}\\) 的過程中，state 的變化同時包含「action 造成的改變」和「隨機或與 action 無關的因素的改變」兩種可能，而後者並不適合作拿來計算好奇心內部獎勵。 
2. 假設 state feature \\(\phi(s_t)\\) 能表現與 action 相關改變的部分（我寫作 \\(\phi(s_t) \overset{a_t}{\rightarrow} \phi({s_{t+1}}) \\)），那麼前後兩個 state feature \\(\phi(s_t)\\) 和 \\(\phi(s_{t+1})\\) 就應該包含一些關於 \\(a_t\\) 的線索（畢竟是 \\(a_t\\) 造成的）。這線索或關聯性就是我們想要透過訓練 \\(\phi\\) 的參數學會的（屬於 inverse model 訓練的一部分）。
    * 或是反過來想，假設 \\(\phi(s) \rightarrow \phi(s_{t+1}) \\) 的變化其實跟你採取什麼 action 完全無關（例如只是環境背景的隨機光影），那麼我們也就難以從他們身上找到能讓我們反推 \\(a_t\\) 的線索。
3. 因此，訓練 inverse dynamics model 反推 \\(a_t\\) 也就是在訓練它找出 state 裡與 action 相關的 feature。

#### Self-supervised Prediction 是什麼意思？

* Self-supervised learning 其實就是一種監督式學習，差別在於一般 supervised learning 使用的答案通常是額外人工標記的，而自監督式學習是利用 input 內隱含的脈絡資訊或是關聯性作為指導訊號來進行監督式學習。
* 本文裡 Self-supervised prediction 指得應該是 inverse dynamics model 學習預測 action 時，正確答案（採取的 action）是自然存在脈絡中、不需額外人工標記的。
* 參考：[What is Self-Supervised Learning?](https://hackernoon.com/self-supervised-learning-gets-us-closer-to-autonomous-learning-be77e6c86b5a)

#### ICM：一些細節

* ICM 的 inverse dynamics model 包含兩個步驟：
    1. 根據 state 產出 feature encoding： \\( s_t \rightarrow \phi(s_t) \\) 
    2. 從兩個連續的 feature encoding 反推 agent 採取的 action。在互動之後我們有 \\(s_t, a_t \rightarrow s_{t+1}\\)，反過來推理：\\( \phi(s_t), \phi(s_{t+1}) \rightarrow a_t \\) 
* 上述兩部分綜合起來便構成 **ineverse dynamics model** \\(g\\)： \\[ \hat{a} = g(s_t, s_{t+1}; \theta_I) \\] 
    * 可透過最小化預測的 loss \\(L_I\\) 來訓練模型參數 \\(\theta_I\\)
    \\[ \underset{\theta_I}{\min} L_I(\hat{a}, a_t) \\]   

* 另一方面，**forward dynamics model** 根據當下的 state feature 與採取的 action 預測未來的 state vector： \\[\hat{\phi}(s_{t+1}) = f\left( \phi({s_t}), a_t; \theta_F  \right) \\] 
    * 可透過最小化 state encoding 預測的 loss \\(L_F\\) 來訓練模型參數 \\(\theta_F\\)：
    \\[ \underset{\theta_F}{\min} L_F \\] 
    \\[ L_F\left( \hat{\phi}(s_t),\phi(s_t) \right) =\frac{1}{2}||\hat{\phi}(s_{t+1}) -\phi(s_{t+1}) ||^2_2 \\]
* **Intrinsic reward signal**，定義為 Forward model 預測 state feature 的誤差： 
    \\[ r^i_t = \frac{\eta}{2}||\hat{\phi}(s_{t+1}) - \phi(s_{t+1}) ||^2_2 \\]
    * \\(\eta>0\\)：scaling factor

* 流程
    * agent 根據當下 state 和自己的 policy \\(\pi(s)\\) 以最大化 intrinsic reward + external reward 的方式選擇 action，與環境互動後進入到下一個 state。 \\( (s_t, a_t, s_{t+1}) \\) 這三項資訊便被用來同時訓練 forward 和 inverse 兩組網路。
    * Inverse 網路學習出一組「跟 action 相關」的 representation （encoding）。
    * Forward 網路學習從這一組「有用的」feature representaion （而不是 raw state）預測未來的 state。

* 綜合 agent training 目標可以寫成：\\[ \underset{\theta_P, \theta_I, \theta_F }{\min} \left[ -\lambda\mathbb{E}_{\pi(s_t;\theta_P)}[\sum_t r_t] + (1-\beta)L_I + \beta L_F \right] \\]
    * 其中 \\(\lambda > 0 \\) 調整學習 policy 和學習 intrinsic reward signal 的相對重要性，\\(0\le\beta<1\\) 調整 inverse 和 forward dynamics model 的重要性。
    
    
#### 實驗檢查

* 使用 random noise source (uniform, Gaussian and Laplacian) 取代 curiosity network 進行測試，確實在 *VisDoom* sparse reward 實驗中變得無法學起來，確認 curiosity 的功能（不是隨機給亂數也行得通）。
    
    
## 小記

* 透過訓練從先後 state feature 反推 action 的方式學習出適合好奇心獎勵的 feature 頗有巧思。這樣學習找出的是相關性還是因果關係？跟 action 無關但會影響 agent 的環境因素找得出來嗎？
* 好奇心內部獎勵跟環境獎勵本質上並不相同，前者並不知道何謂成功，只是鼓勵多嘗試沒做過的部分（避免重複已經知道後果的動作）。這對獎勵稀少的問題來說可能是一個好策略。不過在外部獎勵充足的情況下這樣也許反而降低了學習效率？


### 待理解

* A3C Mnih et al. 2016. asynchronous advantage actor critic policy gradient 的 advantage 部分。
* TRPO-VIME variational information maximization agent trained with TRPO (Houthooft et al., 2016)？