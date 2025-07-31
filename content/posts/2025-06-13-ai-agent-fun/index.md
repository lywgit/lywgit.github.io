+++
date = '2025-06-13T00:00:00+08:00'
title = 'AI Agent Fun'
tags = ["tech", "ai-agent", 'zh']
+++

近期在使用 [Boot.dev](https://www.boot.dev/) 學習一些有興趣的主題（主要是 Go 和後端知識），上週注意到這個新推出的小課程："[Build an AI Agent in Python](https://www.boot.dev/courses/build-ai-agent-python)"，忍不住開始玩，結果變成我在 boot.dev 第一個完成的 Lesson （完成的 code [github link](https://github.com/lywgit/bootdev_ai_agent_in_python)，簡單加上 readme）。

這個 project 的目標是利用現成的 LLM (Gemini API) 來實現「讓 Agent 幫你修復程式 bug」這件事。LLM 能寫 code 能 debug 已不是新聞，類似 github copilot 這樣的功能也已經很成熟，所以我原本並沒有覺得太稀奇，但沒想到完成專案的過程中心裡其實蠻震撼的。

事後回顧，應該是因為這是第一次切身體會到 AI 作為「輔助」和作為「代理」的差異。

Github copilot 這樣的工具正如其名是個輔助駕駛，你才是正駕駛，工作流程的設計也都是 AI 給出建議然後你決定後是否採用。但在這個專案中你的 Agent 一但收到你賦予的問題或任務，就會直接照他的想法開始動作直到他覺得完成了，這之中也包含檔案的新增和修改。

下面是我跟著課程第一次嘗試請 Agent 修 bug 時直接被嚇傻後截的圖。當時下的指令大概是：「calculator 程式有 bug， 3 + 7 * 2 不應該等於 20，請修復」（實際是英文，沒試過中文）

這是一個手動加入的沒有先乘除後加減的 bug，用來測試 Agent。沒想到... 他一出手卻是把整個 caculator/main.py 直接改成兩行

```
result = 3 + (7 * 2) 
print(result) 
```

然後還回覆我「現在結果正確了」？！

![vscode screenshot](vscode-screenshot.png "calculator 主程式的前後 diff 截圖，左為修改前，右邊是Agent 修改後")
 
這個瞬間，我也理解到為什麽很多 vibe coding 的心得文都要強調要用 git 記錄修改的結果、要不斷 commit 之類的。因為如果沒有備份，可能一個沒弄好就把造好的東西給砸了。

當下我只覺得 Gemini 你真是天才... 直到

仔細檢查後，才發現原來我才是那個”天才“... 原來我忘記把程式後的執行結果回傳給 LLM 作為脈絡，導致 Agent 得不到任何他執行行動後的實際反饋。他知道我有給他 tool 可以用於查看檔案內容，也確實試圖查看檔案，但他一直沒有真的看到檔案的內容（因為我忘了印結果給他），最後...Agent 就決定自己胡搞瞎搞了。

修正這個錯誤之後，Agent 也就真的把這個 bug 給看出來且修好了。Pretty impressive!

心得：

1. 代理跟輔助是很不同的概念，可以感受到 Agent 的潛力
2. Agent 的能力越大，Human 的責任就越大
3. 因為相關的能力是自己一步一步賦予給 Agent -> No magic, and it works! 感覺和 copilot 這樣的 coding tool 似乎親近了一些 


---
*文章原發表於 linkedin，後轉移至此*
