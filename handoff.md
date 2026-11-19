# 交接檔（handoff.md）

> 任何 Agent、任何電腦接手前**必讀**；收工時**必更新**。本檔只放交接必需的精簡資訊，詳細脈絡放 Obsidian（`日本道路交通振動評估APP/專案工作流程.md`）。

## ⏯️ 目前做到哪
位置地圖加「量距離」：基本輸入（敏感受體／運輸道路／振動測點）與單筆計算共 4 張地圖卡都有。Google 嵌入地圖不支援量測，按「量距離」會切換成 Leaflet 地圖（OSM／Esri 衛星圖、Nominatim 定位，皆免金鑰），在圖上逐點點選顯示累計距離，支援復原／清除／衛星切換，「結束量測」切回 Google 嵌入地圖。Leaflet 1.9.4 已 vendor 進 `web/vendor/`（雜湊已對過官網 SRI）。
「檔案」分頁（頂部分頁列最左）：專案檔改用 **JSON**（`{app:'roadvib-project', version, store, calcList, batch}`，含基本資料、單筆計算清單、批次設定）。新增（清空三份 localStorage 後重載）、開啟檔案（.json 整案；Excel/CSV 只讀入基本資料）、儲存檔案（File System Access API 回存 .json，檔案代號存 IndexedDB `roadvib-file`，重載後仍可直接儲存）、另存新檔（不支援的瀏覽器改為下載）。介面在 `web/js/file-ui.js`；`store.js` 的 `toWorkbook`（Excel 五表匯出、可往返、含測試）保留備用，目前 UI 未用。
「輸出」分頁（計算與設定之間）：批次的摘要表顯示與匯出整個搬到這裡（計算分頁留資料來源／設定／配對）。匯出 Word、**匯出 Excel（新增）**、匯出 CSV；Excel 依勾選把三張摘要表各寫成一張工作表，名稱比照樣本「輸出表.xlsx」（現況輸出摘要表／施工輸出摘要表／營運輸出摘要表），欄位照 APP 現有版本，數值為數字格式，表註附在表尾。樣本在 `樣本/輸出表.xlsx`（主 checkout）。
登入視窗：開啟程式先要求名稱＋密碼（暫定 CECIHE／12345678，寫在 `web/js/auth.js` 的 `USERS`）。純前端簡易防護——帳密在原始碼與公開 repo 看得到，只擋一般使用者，非資安機制；同分頁登入一次（sessionStorage），關閉分頁重開需再登入。
量測距離自動帶入：敏感受體地圖卡量到距離（≥2 點）後按「新增為敏感受體」，量得的總長（公尺，取到小數 1 位）會自動填入新列的「與基準點距離」欄；量測不足兩點或按復原／清除後不帶入（實作：app.js 把距離寫在 `card.dataset.measureM`，data-ui.js 新增列時讀取）。

## 🚦 目前狀態
可運行。`npm test` 39 項全過；瀏覽器實測量距離（定位、點選量測、多點累計、衛星切換、結束量測）正常。
公式已依第 4.5 節定案修正，平面高架併行結果會與舊 Excel 不同（預期）。

## ➡️ 下一步
1. 使用者確認「營運摘要表」欄位對應（第 4.6 節標示暫定）
2. 使用者用實際專案 xlsm 試用批次功能
3. ~~發佈~~ 已上線 GitHub Pages：https://he22527.github.io/japan-road-vibration/（repo 已轉公開，push 到 main 約 1 分鐘後自動更新）

## ⚠️ 注意事項
- 專案放在 OneDrive 同步資料夾，git 已設定 `windows.appendAtomically false`
- xlsm 為原始參照，請勿直接修改（`web/test/batch.test.js` 直接讀它做驗證）
- 這台電腦的環境變數 `NODE_TLS_REJECT_UNAUTHORIZED=0` 會讓 npm 不驗證 HTTPS 憑證；下載套件後要另外核對雜湊

## 🕐 最後更新
- 時間：2026-11-18
- 更新者：Claude Code @ PC10972（worktree 分支 claude/sensitive-receptor-image-upload-e8d90b）
- Git push：✅ PR #2（量測距離帶入＋檔案分頁）、PR #3（JSON 專案檔＋登入視窗）、PR #4（檔案功能鍵改左側欄）、PR #5（側欄靠左＋刪多餘說明）、PR #6（新增「輸出」分頁占位）、PR #7（輸出分頁搬入摘要表＋匯出 Excel）皆已於 2026-11-19 合回 main；GitHub Pages 會自動更新
