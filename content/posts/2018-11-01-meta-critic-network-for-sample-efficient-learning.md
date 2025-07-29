+++
date = '2018-11-01T00:00:00+08:00'
title = 'Meta-Critic Networks for Sample Efficient Learning'
tags = ['read', 'ml', 'zh']
+++

> 延伸強化學習中的 actor-critic 架構，用同一個 meta-critic 指導許多同類型的不同工作，目標是訓練一個可以有效指導新 few-shot 訓練問題的 meta-critic。為了讓 meta-critic 可以區別不同 task 的差異，必須把「學習軌跡（learning traces）」encode 後也作為 meta-critic 的輸入。
> 
> 因此 Meta-critic 包含兩部分：（1）對應傳統的 value function 的 Meta-Value Network，以及（2）負責 encode 學習軌跡（state, action, reward 的歷史）再輸入給 MVN 的 Task-Action Encoder Network。實驗於 sine & linear function regression（監督式學習）以及 cartpole control 遊戲問題（強化學習）。


| 文章 |     |
|-----|-----|
| 標題 | Learning to Learn: Meta-Critic Networks for Sample Efficient Learning |  
| 作者 | Flood Sung, Li Zhang, Tao Xiang, Timothy Hospedales, Yongxin Yang (2017) |
| 連結 | [arXiv:1706.09529](https://arxiv.org/abs/1706.09529) |

## 主要思路

#### 一、問題

* AI 沒辦法很「有效率」的學習新的工作
    * 對監督式學習（SL）來說，這表示訓練一個新模型通常需要很多資料（data）。
    * 對強化學習（RL）來說，這表示需要與環境互動非常多次（interaction）。

#### 二、目標

* 讓一個 meta-learner (ceta-critic) 在特定 Class（但不相同的）許多工作上訓練 learner (actor)，藉此學會關於此問題群的特性。
* 著重在 few-shot learning 的改善：往後在面對（同 Class 的）新問題時，即使樣本很少（SL）或容許互動次數較少（RL），也能有效的訓練出好的網路。

#### 三、方法

* 傳統 critic（一個價值網路）只考慮 state 和 action，現使用一個跨工作的 meta-critic 並把當下 learning tracde 也當作輸入資訊以供區別。
* meta-critic 由兩個 network 組成：
    1. Meta-Value Network（MVN, or value net）：以MLP實作的價值網路。讓 actor 據此調整其行動策略 policy。
    2. Task-Actor Encoder Network（TAEN, or task net）：以 LSTM 實作的 encoder。輸入一系列的學習歷史 (state, action, reward) triplets，輸出 embedded feature 給 MVN 使用 。由於各種問題都會有 state, action, reward，不會限神經網路使用的構運。
   
#### 四、結果

1. SL：在人工合成的 \\(a\sin(x+b)\\) 與 \\(cx+d \\) 函數集資料上作 Regression 問題（ground truth函數皆為sine 和 linear，但不同 task 的 \\(a,b,c,d\\) 不同）。meta-learning 在此對應到的是學會 sine 函數及直線的一班特性。 
    * 訓練好的 meta-critic 在測試函數上能夠從很少筆資料點（例如 4-shot）估計出接近 gound truth 的 \\(a,b,c,d\\)，表示 meta-training 時確實有學到東西。 
2. RL：在 Cartpole Control 問題上，使用 meta-critic 學習得更快（reward 的進步 per training episode 更高），最終測試中的 success rate 也比其他方法高出許多。
    * 有趣的是 TAEN 的 embedding \\(z\\) 顯示 meta-critic 學會的區別可以對應到不同長度的 pole length，這並不是 actor 和 critic 知道的資訊！
 
#### 五、貢獻
* 提出一種有彈性的、新的 knowledge transfer 途徑。可運用在 SL 和 RL 的 few-shot learning 問題上。


## 其他概念

### 從 Critic 到 Meta-Critic 

*傳統 RL Actor-Critic 架構*

* actor（參數 \\(\theta\\)）根據 state 輸出 action \\(a_t = P_\theta(s_t)\\)
    * 訓練方式：maximize output of critics
* critic（參數 \\(\phi\\)） 估計 state-action 未來的回報（expected discount reward）
    * 訓練方式：minimise the temporal difference error
* 更新方式：
    * \\( \theta \leftarrow \underset{\theta}{\arg\max} \ Q_\phi(s_t,a_t) \\)
    * \\( \phi \leftarrow \underset{\phi}{\arg\min} \ \left( Q_\phi(s_t,a_t) - r_t  -\gamma Q_\phi(s_{t+1},a_{t+1}) \right)^2 \\)
    
*Meta-Critic MVN + TAEN 的架構*

* critc 本來只以 state 和 action 為 input，現在加入 task-actor encoding network (TAEN) \\(C_\omega\\)（參數 \\(\omega\\)） 來產生學習軌跡的 embedding \\(z_t = C_\omega(L^t_{t-k}) \\)；\\(L\\) 是 actor 的 *learning trace*
    * \\( L^t_{t-k} =[(s_{t-k}, a_{t-k}, r_{t-k}), (s_{t-k+1}, a_{t-k+1}, r_{t-k+1}), \dots, (s_{t-1}, a_{t-1}, r_{t-1})] \\)
        * 透過 reward 知道關於 task 的資訊，透過 state, action 知道 actor 的資訊。
* 更新方式（每 \\(M\\) 個 task 為一組 mini-batch，更新一次 meta-critic 參數）：
    * \\( \theta^{(i)} \leftarrow \underset{\theta^{(i)}}{\arg\max} \ Q_\phi(s_t^{(i)}, a_t^{(i)}, z_t^{(i)} ) \quad \forall i \in [1,2,\dots,M] \\)
    * \\( \phi,\omega \leftarrow \underset{\phi,\omega}{\arg\min} \ \sum_{i=1}^M \left( Q_\phi(s_t^{(i)}, a_t^{(i)}, z_t^{(i)} ) - r_t^{(i)}  -\gamma Q_\phi (s_{t+1}^{(i)}, a_{t+1}^{(i)}, z_{t+1}^{(i)}) \right)^2 \\)


### 從強化學習的角度看監督式學習

* SL 就像只玩一次的 game：
    1. actor 輸入 \\(x\\) 輸出 prediction \\(\hat{y}\\)
    2. environment 給予一次 reward \\(r=-\mathscr{l}(\hat{y}, y)\\) (負的 loss)，然後遊戲就完成了。
* 更新方式： 
    * \\( \theta \leftarrow \underset{\theta}{\arg\max}\, Q_\phi (x, \hat{y}) \\)
    * \\( \phi \leftarrow \underset{\phi}{\arg\min} \left(Q_\phi(x, \hat{y}) - r \right)^2 \\)
        * 其中 \\(\hat{y} = P_\theta(x) \\), and \\( r=-\mathscr{l}(\hat{y}, y) \\)
* 所以 RL 對照 SL：
    1. state \\( s \rightarrow x \\)：state 相當於問題資訊（並無前一次 action）
    2. action \\( a \rightarrow \hat{y} \\)： prediction 就是 policy network 作出的 action。
    3. reward \\( r \rightarrow -\mathscr{l}(\hat{y}, y) \\)：算出來的 loss 越低就是這個 action 的 reward 越好。
* 這種看法其實和傳統 SL 有很不同的假設，因為現在變成 actor 並不知道真值 \\(y\\)、也不知道 loss function 的形式，只知道得到的 reward 多少。Critic 的角色就變成學習估計當下的 reward (因為 game 只有一步)，也因為有 Critic 在，並不需要每一筆資料都有真值。

### 針對 few-shot 實驗的設計

|     | 訓練 | 資料 | 
|------|------|----|
| (i) Meta-trainging | meta-critics + actors | 許多 source tasks <br/> 許多資料點 / 多次數環境互動 |
| (ii) Meta-testing | actors | new tasks <br/> 少數資料點 / 少次數的環境互動  | 
| (iii) Final testing | 無 (僅評估 actor 的能力) | (same) new tasks  <br/> 足夠評分 |


|  |  SL sine/linear 實驗<br/>(task 不同參數) | RL cartpole 實驗 <br/> (task 不同 pole length)|
|------|----|----|
| (i)  | 10,000 source tasks; 各 30,000 的個資料點 | 1000 tasks; 各 150,000 互動 | 
| (ii)  | new tasks; 各 4, 6 或 8 個資料點 | 玩 100 games (倒下或達 200 次動作) |
| (iii)  | new tasks; 各 100 資料點算 MSE  | 玩 10 games 算平均分數 |


<small> 
註：actor 都是每個 task 各自獨立一個。<br/>
註：cartpole 向左或向右動一下算一次互動。 
<small/>



## 小記

1. 概念上是用一個 meta-learner 去學特定一類的問題的共通知識。學會的知識存在 MVN 和 TAEN 中。
2. 和 MAML（Model-agnostic meta-learning for fast adaptation of deep networks）的作法有相似之處，但使用 RL 框架下的 crtic 具有更不受限於問題的 prior distribution 的好處。
3. 特色："Learning to supervise" rather than "learning to synthesize" 

### 待理解

1. 和 semi-supervise learning 的關係。
2. actor-critic 的實作。
3. 疑問：這個 meta-critic 如果用在其他不同類問題上，是不是因為其bias而變得比空白 critic 差？
4. Dirichlet distribution（我略過 Dependant Multi-arm Bandit 實驗）

{{< katex >}}