# Minesweeper Pro｜踩地雷

純 HTML5、CSS3 與原生 JavaScript 的單檔踩地雷遊戲。完整網站在根目錄 `index.html`，無需編譯、後端、CDN、字型服務或付費 API，可直接開啟或放上 GitHub Pages。

## 功能

- 初級 9 × 9／10 雷、中級 16 × 16／40 雷、高級 16 × 30／99 雷。
- 首次挖掘才生成地雷，該格與鄰近八格安全；迭代式洪水填充展開空白區。
- 右鍵插旗、手機挖掘／插旗模式、鍵盤 Tab 與 Enter／空白鍵操作，F 插旗。
- 只在所有安全格開啟後獲勝；結束後鎖定棋盤並停止計時。
- 各難度最佳時間、主題及難度偏好以 LocalStorage 保存；無法儲存時仍可遊玩。
- 深淺主題、數字配色、動畫、減少動態效果支援、寬棋盤水平捲動。

旗標為推測，可超過地雷數，剩餘旗標會顯示負數。未開啟的安全格即使插旗仍不算完成。重新開始或切換難度會重設本局。

## 本機遊玩

直接開啟 `index.html`。若要以 HTTP 測試，在根目錄執行 `python3 -m http.server 8000`，瀏覽 `http://localhost:8000`。瀏覽器的 LocalStorage 在不同網址、隱私模式或停用儲存時可能有不同表現。

## GitHub Pages

網站已成功部署，可直接遊玩：<https://ray-chen1124.github.io/minesweeper-pro/>。

已確認 [Run #3](https://github.com/ray-chen1124/minesweeper-pro/actions/runs/39208589153) 的部署與 `Verify public website` 步驟均成功。該步驟由 GitHub runner 請求公開網址，確認 HTTP 請求成功，並檢查回傳 HTML 包含遊戲標題、棋盤與內嵌遊戲邏輯，沒有外部 script 標籤。

目前 Pages Source 為 **GitHub Actions**。`.github/workflows/pages.yml` 在 main 更新時部署，只上傳 `index.html`，部署後自動檢查公開網址；含 `[skip ci]` 的文件提交不觸發此 push 工作流程。

實際測試與限制見 [TESTING.md](TESTING.md)，開發過程見 [DEVELOPMENT.md](DEVELOPMENT.md)。
