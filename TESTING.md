# 實際測試紀錄

日期：2026-10-19。測試環境：Linux、Chromium、Playwright；以本機 HTTP 載入真正的 index.html。JavaScript 經 `new Function` 語法檢查成功。

## 自動化結果

已實際執行 13 組檢查，13 組通過：

1. 初始 81 格，尚未產雷，狀態 ready。
2. 三種難度各隨機生成 100 局（共 300 局）。逐局驗證雷數、首次格及鄰格無雷；以獨立雙迴圈重算每一格鄰雷數並比對。
3. 真實滑鼠右鍵插旗、左鍵不開啟旗標、不啟動計時、右鍵取消旗標。
4. 真實點擊後，用另一份 BFS 算法比對洪水展開結果；等待超過一秒確認計時器增加。
5. 挖到地雷後為 lost，全部棋盤按鈕停用，程式呼叫也不能改動棋盤、旗標與時間；顯示重試按鈕。
6. 透過重試按鈕重設；受控局面逐一開啟安全格，確認最後一格前仍 playing，全部開啟才 won，不必插旗；紀錄寫入 LocalStorage。
7. 較慢的勝利不覆蓋最佳紀錄，重新載入頁面仍保留。
8. 切換高級立即變為 480 格且重設，主題與難度於重新載入後保留。
9. 洪水展開跳過插旗安全格；即使其他安全格都開啟，仍須取消旗標並開啟該格才勝利。
10. 模擬 Storage.setItem 拋出錯誤，勝利流程仍完成，顯示儲存不可用提示。
11. 390 × 844、觸控模式中，實際 tap 切換插旗、挖掘、取消旗標並成功開局。
12. 390px 高級棋盤可水平捲動，整份頁面寬度無溢出。
13. 未捕捉的瀏覽器 JavaScript 錯誤為零。

部分受控勝負測試透過瀏覽器 evaluate 存取遊戲狀態以建立精確局面；滑鼠／觸控操作另外透過 Playwright 真實事件驗證。測試不是求解器，沒有宣稱所有隨機局均可不靠猜測通關。

已檢視桌面與手機整頁截圖，並修正手機「已開啟安全格」數值換行問題。

## 未執行範圍

未以實體手機、Safari、Firefox 或螢幕閱讀器驗證。手機測試為 Chromium 觸控模擬。GitHub Pages 部署與公開 HTTP 狀態另以實際工作流程結果記錄，不能由本機測試推論成功。

## GitHub Pages 部署與公開網址驗證

目前狀態：**已成功部署，公開網址 HTTP 與內容檢查已通過**。

- 遊玩網址：<https://ray-chen1124.github.io/minesweeper-pro/>。
- 驗證工作流程：[Run #3](https://github.com/ray-chen1124/minesweeper-pro/actions/runs/39208589153)。
- 對應 commit：`12290b04ec375bfcc302496dc84ce13daba53539`。
- 已確認結果：configure-pages、artifact 上傳、Deploy 與 `Verify public website` 步驟全部成功。

### 公開網址檢查方法與實際結果

GitHub runner 使用 curl 請求部署輸出的公開 URL，跟隨重新導向並以失敗 HTTP 狀態作為錯誤；失敗時最多重試六次。接著使用 Python 檢查回傳 HTML 包含遊戲標題、棋盤元素與內嵌地雷生成邏輯，且沒有外部 script 標籤。Run #3 中此步驟已實際執行並成功。

此檢查驗證公開 HTTP 存取與 HTML 內容；遊戲互動仍以先前已執行的 Chromium／Playwright 測試為依據。當時工作環境及網頁讀取工具無法直接連線至公開網址，公開存取結果來自 GitHub runner，未宣稱在本機瀏覽器重新測試線上互動。

### 部署歷程

首次部署曾在 `actions/configure-pages@v5` 失敗。使用者調整 Pages 設定後，[Run #2](https://github.com/ray-chen1124/minesweeper-pro/actions/runs/39206971270) 與 [Run #1 的重試](https://github.com/ray-chen1124/minesweeper-pro/actions/runs/39206853096) 均部署成功；後續 Run #3 另確認公開網址及內容檢查成功。初次失敗為已解決的歷史紀錄，目前無需再啟用 Pages 或重新執行部署。

本次僅更新 README.md 與 TESTING.md 的部署說明，並核對上述既有工作流程結果；未修改遊戲或重新執行遊戲測試。
