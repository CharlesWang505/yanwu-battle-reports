# 三國：謀定天下演武大會 S7-S16 戰報庫

[简体中文](README.md) | 繁體中文

本儲存庫公開《三國：謀定天下》演武大會 S7-S16 的結構化戰報資料，以及將戰報截圖整理為統一文字記錄時使用的 AI 提示詞。

## 資料集

資料按賽季獨立存放在 [`data/`](data/) 目錄中。檔案來自網站公開介面，已移除原始 OCR 文字、截圖、使用者提交資料、帳號資料、存取憑據和伺服器端設定。
部分戰報來源：https://github.com/liamqma/sanmou-yanwu/tree/master/data/battles

| 賽季 | 戰報數量 | 檔案 |
| --- | ---: | --- |
| S7 | 425 | [`data/S7/S7-battle-reports-20260818.ywrlib.json`](data/S7/S7-battle-reports-20260818.ywrlib.json) |
| S8 | 985 | [`data/S8/S8-battle-reports-20260818.ywrlib.json`](data/S8/S8-battle-reports-20260818.ywrlib.json) |
| S9 | 1,319 | [`data/S9/S9-battle-reports-20260818.ywrlib.json`](data/S9/S9-battle-reports-20260818.ywrlib.json) |
| S10 | 1,930 | [`data/S10/S10-battle-reports-20260818.ywrlib.json`](data/S10/S10-battle-reports-20260818.ywrlib.json) |
| S11 | 3,050 | [`data/S11/S11-battle-reports-20260818.ywrlib.json`](data/S11/S11-battle-reports-20260818.ywrlib.json) |
| S12 | 4,077 | [`data/S12/S12-battle-reports-20260818.ywrlib.json`](data/S12/S12-battle-reports-20260818.ywrlib.json) |
| S13 | 5,813 | [`data/S13/S13-battle-reports-20260818.ywrlib.json`](data/S13/S13-battle-reports-20260818.ywrlib.json) |
| S14 | 6,702 | [`data/S14/S14-battle-reports-20260818.ywrlib.json`](data/S14/S14-battle-reports-20260818.ywrlib.json) |
| S15 | 7,443 | [`data/S15/S15-battle-reports-20260818.ywrlib.json`](data/S15/S15-battle-reports-20260818.ywrlib.json) |
| S16 | 8,154 | [`data/S16/S16-battle-reports-20260818.ywrlib.json`](data/S16/S16-battle-reports-20260818.ywrlib.json) |

格式為 `yanwu-report-library-public` v1。每筆記錄保留戰果、左右陣型、武將、戰法和剩餘兵力等結構化欄位。

S1-S6 不包含在本公開戰報資料集中；S1-S2 的經典推薦資料不屬於本儲存庫的主要戰報庫。

## 使用提示詞

批次辨識戰報截圖時，使用 [prompts/battle-report-ocr.zh-TW.md](prompts/battle-report-ocr.zh-TW.md) 中的提示詞。提示詞限定單批最多 20 張圖片，並要求逐行輸出固定結構，方便後續匯入與校驗。

## 資料結構

頂層物件範例：

```json
{
  "format": "yanwu-report-library-public",
  "version": 1,
  "season": "S16",
  "exportedAt": "2026-08-18T00:00:00.000Z",
  "reports": []
}
```

`reports` 內每筆記錄使用以下主要欄位：

```json
{
  "id": "UUID",
  "season": "S16",
  "parsed": {
    "winnerSide": "left | right | 空字串",
    "leftTeam": {
      "formation": "陣型名稱",
      "heroes": [{
        "name": "武將名稱",
        "tactics": ["戰法名稱"],
        "remainingTroops": { "current": 0, "max": 0 }
      }]
    },
    "rightTeam": {}
  }
}
```

## 校驗

```powershell
Get-FileHash -Algorithm SHA256 .\data\S16\S16-battle-reports-20260818.ywrlib.json
node -e "const fs=require('fs');const j=JSON.parse(fs.readFileSync('data/S16/S16-battle-reports-20260818.ywrlib.json','utf8'));console.log(j.reports.length)"
```

## 授權與聲明

本儲存庫中由維護者整理的資料結構、提示詞和文件按 [CC BY 4.0](LICENSE) 發布。遊戲名稱、角色名稱、戰法名稱和其他相關內容的權利歸各自權利人所有；本儲存庫與遊戲發行方沒有隸屬或授權關係。

請自行確認資料使用符合適用法律、平台規則和相關權利人的要求。提交新資料前，請移除截圖、帳號、聯絡方式、裝置識別碼、存取憑據和其他個人或敏感資訊。
