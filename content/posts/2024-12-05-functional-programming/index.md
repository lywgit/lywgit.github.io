+++
date = '2024-12-05T00:00:00+08:00'
title = '簡約軟體開發思維：用Functional Programming 重構程式'
tags = ["tech", "read", "software", "zh"]
+++

最近正好對軟體的複雜度有感，在看到「簡約軟體開發思維：用 Functional Programming 重構程式」（Eric Normand）一書時，就被標題 Taming complex software 吸引到。簡單翻看後直覺區分 Action、Calculation、Data 會是個有用的觀念，對於多執行序的一些分析也會是很好的思考材料，於是花時間一讀。

對任何寫程式的人來說 function 一定都沒有少用過，不過我對 Functional Programming 一詞卻也一直僅是耳聞。本書的作者強調從工程師的角度出發介紹 FP 中實用的觀念而不討論學術和理論的部分，大概包含幾項重點： (1) 要認識 calculation，也就是沒有副作用的 pure function 的好處，(2) 要運用 copy-on-write 保持 data 不變 (3) 把 function 當作頭等物件來運用，以及 (4) 透過時間線分析多執行緒情境下的 Action。

相對於「領域區驅動設計」一書來說，本書比較沒那麼硬，大部分重構的步驟也蠻自然的，學到不少。當然，看是一回事，用又是另一回事...。也許哪天再來看看 Python 的 functools 套件支援了什麼樣的操作吧。

---

![note](fp-note.png "筆記<br>(Right click -> Open Image in New Tab to view original-size image)")

![takeaway](fp-takeaway.png "心得<br>(Right click -> Open Image in New Tab to view original-size image)")


---
*文章原發表於 linkedin，後轉移至此*

