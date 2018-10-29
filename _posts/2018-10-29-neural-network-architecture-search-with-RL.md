---
title: Neural Architecture Search with RL 
category: paper-note
tag: [network-architecture, reinforcement-learning, zh]
---

訓練一個 Controller（LSTM）來自動設計好的神經網路架構。Controller 負責輸出子模型架構的超參數，建立的子模型在資料上訓練後計算 validation accuracy，以此為 reward 回饋給 Controller 參考。Controller 應用強化學習的 policy gradient 方法修正自己，讓下一次產生的架構能更好（產生的子網路 validation accuracy 更高）。

| 文章 |  |
|-----|-----|
| 標題 | Neural Architecture Search with Reinforcement Learning |  
| 作者 | Barret Zoph, Quoc V. Le (2016) |
| 連結 | [arXiv: 1611.01578](https://arxiv.org/abs/1611.01578)| 


## 主要思路

#### 一、問題
* 神經網路取得許多成功，但依賴人工設計好的架構，耗時不易。

#### 二、目標
* 找出好的神經網路架構 （Neural Architecture Search NAS）：
    * CIFAR-10（圖像）：找好的 CNN 架構
    * [Penn Treebank](https://web.archive.org/web/19970614160127/http://www.cis.upenn.edu/~treebank/)（語言）：找好的 RNN 單元架構

#### 三、方法

* 神經網路的架構（各種 hyperparameters）可以用長度不等的一連串 token 表示，使用一個 RNN 作為 controller 負責產生這些 token，就可以建立出對應的子模型。
* 子模型在 training data 上訓練，並在 validation data 上計算 accuracy。
* 以此 accuracy 作為 reward，讓 controller 用 policy gradient 的方法學習（更新自己的參數 \\(\theta_c\\)），使下一次有更高機會產生更好的架構。  
 
#### 四、結果

* 在 CIFAR-10 資料集上，找出的 CNN 子網路模型可以得到媲美人工設計的網路架構的 test accuracy。
* 在 Penn Treebank 資料集上設計了全新的 RNN cell 架構，語言模型的困惑度（perplexity）比之前最佳紀錄更好。

#### 五、貢獻

* 讓機器自行搜尋好的神經網路模型架構（自動化）。
* 可以找出較不受限於人類思考框架的架構。

## 其他觀念

#### Controller 

* Controller 輸出的序列被視為一連串的 action：\\(a_{1:T}\\)
* Controller 的訓練目標目標是最大化 reward \\(R\\)（validation set 的 accuracy）的期望值： 
\\[ J(\theta_c) = E_{P(a_{1:T};\theta_c)}[R] \\]
    * 因為沒辦法微分，必須使用 policy gradient。採用 the REINFORCE rule from Williams (1992) 
\\[ \nabla_{\theta_c} J(\theta_c) = \sum_{t=1}^T E_{P(a_1:a_T;\theta_c)}[\nabla_{\theta_c} \log(P(a_t|a_{(t-1):1};\theta_c) R ]  \\] 

#### Skip connection (CNN)

* 在每個 layer 插入 anchor point，用 set-selection attention 來決定每一層（的 anchor）是否要和之前任一層（的 anchor）形成輸入關係。如果有許多輸入層，則往深度方向疊起來作為 input。
* 遇到輸入層間不相容或是沒有輸入、輸出層時的規則：
    1. 沒有輸入的話用 image 做輸入
    2. 最後一層會把所有沒有對應到輸出層的 output 疊起來也送到分類器      
    3. 如果形狀不相符，小的zero padding 到大的再疊

#### Forming RNN

* RNN cell 架構可以用 tree 的方式分層描述，對任兩筆 input 需要決定
    1. combination method：{`addition`, `elem_mult`, etc.}
    2. activation method：{`identity`,`tanh`, `sigmoid`,`relu` etc.}
* 效法 LSTM 額外有 memory state 的架構要決定。
 
#### Controll experiment

* 和 Random search 比較，policy gradient 確實更有效率的找到好的網路架構。
 
 
## 小記

* 需要強大運算 power，因為必須訓練很多很多不同架構子模型。每次算 gradient 也都要抽樣子模型計算。
* 框架還是有一些限制，例如 filter size 只給選 [1,3,5,7] 而不是真的任意。應該是因為 search space 實在太大。 


### 待理解

* policy gradient 的運作
    * The REINFORCE rule from Williams (1992)?
    * 加 baseline function 降低 variance？
* attention mechanism (Bahdanau et al., 2015; Vinyals et al., 2015)
* set-selection type attention (Neelakantan et al., 2015)
