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

## GitHub Pages 實際部署結果

程式已提交至 main，commit `da9fba51d1988966bce78a6d8e15cb3ae1aac46b`。工作流程 [Run #1](https://github.com/ray-chen1124/minesweeper-pro/actions/runs/39206853096) 實際執行，checkout 成功，`actions/configure-pages@v5` 失敗，之後的上傳與部署步驟未執行。可用連接器無法取得該步驟完整日誌，因此不將確切失敗原因當成已確認事實。

尚未確認公開網站可存取。下一步：Repository → Settings → Pages → Source 選擇 **GitHub Actions**，然後在 Actions 重新執行部署。現有連接器沒有可修改 Pages 設定的操作。

## Pages 設定修正後

使用者調整 Pages 設定並重新執行後，[Run #2](https://github.com/ray-chen1124/minesweeper-pro/actions/runs/39206971270) 已成功完成 configure、artifact 上傳與 Deploy，Run #1 的重試亦成功。此前失敗紀錄保留供開發歷程參考。

目前執行環境及網頁讀取工具無法直接連線至公開網址，因此新增部署後的 `Verify public website` 步驟：由 GitHub runner 以 curl 請求部署輸出的 URL（失敗時最多重試六次），再檢查回傳 HTML 具備遊戲標題、棋盤與內嵌生成邏輯，且沒有外部 script 標籤。此步驟只驗證公開 HTTP 及檔案內容，互動功能仍以先前 Chromium 測試為依據。實際執行結果以最新 Actions run 的該步驟為準。
