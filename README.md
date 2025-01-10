# Vue 3 + Vite Todo List

使用 Vue 3 和 Vite 開發 Todo List。專案中採用了 Vue 3 的 `<script setup>` Composition API，使用 Vite 作為開發工具。UI 部分則使用了 Tailwind CSS 來實現響應式設計，並引入了 SweetAlert2 做彈窗提示，讓使用者互動體驗更好。

## 使用技術
- **Vue 3**：使用組合式 API 實現組件邏輯。
- **Vite**：作為開發工具，快速的編譯與熱重載，提升開發效率。
- **Tailwind CSS**：快速開發響應式 UI，utility-first靈活設定樣式。
- **SweetAlert2**：使用於彈窗，用於提示日期區間的部份。

## 主要功能
- **待辦事項管理**：使用者可以新增、查看、刪除待辦事項。
- **動態增加項目**：使用者最多可新增十個項目，超過時按鈕將被禁用。
- **圖片功能**：提供隨機圖片、上傳圖檔、插入圖片連結等新增方式，新增後會即時顯示圖片預覽。
- **響應式設計**：支援PC與Mobile。

## 安裝與使用

- 安裝
```bash
git clone https://github.com/your-username/vue-todolist.git
```
- 啟動
```bash
cd /VueToDoList
npm install
npm run dev
```