# IMI PDF Studio 升級｜前置驗證與驗收門檻（2026-11-01）

> 僅供 upgrade/a-h-t-p-20261101 測試分支。正式 main 與公開網站不得更動，除非曾老師另行確認。

## 基線
- 倉庫：IMIEvelyn/imi-pdf-studio；起點：2026-11-01 main。
- 現行單頁 index.html：React 18、Tailwind、瀏覽器 Babel、Gemini REST。
- 已檢查：tryLocalParse、handleGeneratePreview、InlineRubyText、print CSS、window.print。
- 目前 preview 使用單一長 DOM #imi-print-area；列印另有 @media print 和 page-break 規則。不能宣稱預覽與下載 PDF 真正同版，須做 PoC。
- 目前模型字串 gemini-3.6-flash；現有 fetchWithBackoff 會自動重試，違反已確認缺漏不得自動重送要求。

## 先行 PoC：P 類
1. A4 210×297 mm 真實多頁與每頁獨立邊框；預覽能逐頁查看。
2. 保留現有 InlineRubyText 視覺樣式、右側直式注音、輕聲點位置、字下拼音和原色彩，必須以截圖比對。
3. 每筆詞卡不拆；對話句子不任意折頁；標題不留在頁尾；不縮字、不裁切。
4. 以同一排版資料產生預覽與列印／下載；確認選取複製中文字正常，及列印字型、標音不失真。
5. 測試極長課文、跨頁對話、外語超長文字、空模組、手機寬度與縮放。
6. 若瀏覽器列印無法提供真正同一 PDF 檔案預覽，先回報技術限制與替代實作，不得偷偷將點陣圖視作等價 PDF。

## Gemini 模型驗證
- 候選：gemini-3.8-flash、gemini-3.5-flash-lite；舊版 gemini-3.6-flash 僅作比較基線。
- Google 官方模型文件列出候選且宣稱支援 structured outputs。
- 在沒有教師明確授權使用 API Key 前，僅做文件、請求格式與模擬回應測試；不得宣稱完成真實翻譯品質比較。
- 對相同詞彙與五模組測試 JSON schema、stable item IDs、語言名稱、POS 翻譯、缺漏保留；記錄時間、輸入／輸出 token 與可用性僅作工程評估，不建立教師使用 Token 紀錄功能。
- 模型名稱固定在集中設定，不自動跳新模型；變動前重新驗證。

## 六批驗收
1. GEM 匯入：任意非空模組、空欄位／損壞項目保留，成功解析不受其他錯誤影響。
2. 本地 POS 與拼注音：無 API/安裝；正簡依輸入；一、不變調；第三聲標字典本調；保留原顯示。
3. 教師校正：人工最高；完全相同斷詞跨五模組同步；個別出現位置例外，不被全域覆蓋。
4. API 翻譯：詞彙及外語 POS 必翻，其餘依教師選取；集中請求／必要時分批；單次 Key；失敗不自動重試；可手填／主動補翻／略過。
5. 自訂語言、當前教材自動暫存（不存 Key）、側邊欄及真正 PDF 分頁／下載；內容與風格不變。
6. 端到端測試、下載 PDF 視覺迴歸、手機和列印測試、獨立測試連結；老師批准後才可合併 main。

## 發布保護
- 所有改動只寫入 upgrade/a-h-t-p-20261101。
- 不將分支部署到原正式網址；測試站須使用獨立位址。
- 未有老師最後明確同意，不得合併或部署正式版。

## 前置狀態
- [x] GitHub 主倉庫權限與 default_branch 確認
- [x] 專用測試分支建立
- [x] 原碼靜態檢查與現有 PDF/模型風險記錄
- [x] Google 官方模型清單與 structured-output 文件初步核對
- [ ] 實機 PDF PoC、視覺比對及 PDF 文字可選取驗證
- [ ] 模擬請求及實際 API 品質比較（實際 Key 需另行授權）
- [ ] 六批程式實作及 QA
