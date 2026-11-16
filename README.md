# 三国：谋定天下演武大会 S7-S17 战报库

[繁體中文](README.zh-TW.md)

本仓库公开《三国：谋定天下》演武大会 S7-S17 的结构化战报数据，以及将战报截图整理为统一文本记录时使用的 AI 提示词。

## 数据集

数据按赛季独立存放在 [`data/`](data/) 目录中。当前快照基于维护者提供的 2026-11-09 战报导出文件生成，已去除原始 OCR 文本、截图、用户提交信息、账号信息、访问凭据和服务端配置。旧版 `20260818` 快照保留，下面列出当前版本。
部分战报来源：https://github.com/liamqma/sanmou-yanwu/tree/master/data/battles

| 赛季 | 战报数量 | 文件 |
| --- | ---: | --- |
| S7 | 438 | [`data/S7/S7-battle-reports-20261109.ywrlib.json`](data/S7/S7-battle-reports-20261109.ywrlib.json) |
| S8 | 992 | [`data/S8/S8-battle-reports-20261109.ywrlib.json`](data/S8/S8-battle-reports-20261109.ywrlib.json) |
| S9 | 1,352 | [`data/S9/S9-battle-reports-20261109.ywrlib.json`](data/S9/S9-battle-reports-20261109.ywrlib.json) |
| S10 | 1,922 | [`data/S10/S10-battle-reports-20261109.ywrlib.json`](data/S10/S10-battle-reports-20261109.ywrlib.json) |
| S11 | 3,026 | [`data/S11/S11-battle-reports-20261109.ywrlib.json`](data/S11/S11-battle-reports-20261109.ywrlib.json) |
| S12 | 3,991 | [`data/S12/S12-battle-reports-20261109.ywrlib.json`](data/S12/S12-battle-reports-20261109.ywrlib.json) |
| S13 | 5,650 | [`data/S13/S13-battle-reports-20261109.ywrlib.json`](data/S13/S13-battle-reports-20261109.ywrlib.json) |
| S14 | 6,581 | [`data/S14/S14-battle-reports-20261109.ywrlib.json`](data/S14/S14-battle-reports-20261109.ywrlib.json) |
| S15 | 7,599 | [`data/S15/S15-battle-reports-20261109.ywrlib.json`](data/S15/S15-battle-reports-20261109.ywrlib.json) |
| S16 | 8,017 | [`data/S16/S16-battle-reports-20261109.ywrlib.json`](data/S16/S16-battle-reports-20261109.ywrlib.json) |
| S17 | 8,248 | [`data/S17/S17-battle-reports-20261109.ywrlib.json`](data/S17/S17-battle-reports-20261109.ywrlib.json) |

格式为 `yanwu-report-library-public` v1。每条记录保留战果、左右阵型、武将、战法和剩余兵力等结构化字段。

### 筛选口径

2026-11-16 更新使用 `yanwu-tool-open` 的赛季武将库、战法库及自带战法名称，重新筛选 S7-S17，不按原始记录的 S16/S17 标签直接分组。双方各须有 3 名武将，全部武将和携带战法均须存在于目标赛季库；同一战报可以进入多个适用赛季，输出记录的 `season` 表示适用赛季，并不证明实际对战发生于该赛季。

原始导出共 10,282 条，按战果、双方阵型、武将、战法和剩余兵力去重后为 10,079 条；重复项保留更新时间最新的记录。名称只去除首尾空白，不猜测修正 OCR、别名或标点。S17 严格筛选后保留 8,248 条，其余 1,831 条包含无法匹配的武将或战法；部分历史影本不在当前战法库中，也会排除。因此本次数量可能低于旧版，且不代表完整历史战报数量。

快照日期 `20261109` 对应原始导出在北京时间的日期；`exportedAt` 保留原始导出时间 `2026-11-08T17:05:22.229822+00:00`，不是本次整理时间。

S1-S6 不包含在本公开战报数据集中；S1-S2 的经典推荐资料不属于本仓库的主战报库。

## 使用提示词

批量识别战报截图时，使用 [prompts/battle-report-ocr.md](prompts/battle-report-ocr.md) 中的提示词。提示词限定单批最多 20 张图片，并要求逐行输出固定结构，便于后续导入和校验。

## 数据结构

顶层对象示例：

```json
{
  "format": "yanwu-report-library-public",
  "version": 1,
  "season": "S17",
  "exportedAt": "2026-11-08T17:05:22.229822+00:00",
  "reports": []
}
```

`reports` 内每条记录使用以下主要字段：

```json
{
  "id": "UUID",
  "season": "S17",
  "parsed": {
    "winnerSide": "left | right | 空字符串",
    "leftTeam": {
      "formation": "阵型名称",
      "heroes": [{
        "name": "武将名称",
        "tactics": ["战法名称"],
        "remainingTroops": { "current": 0, "max": 0 }
      }]
    },
    "rightTeam": {}
  }
}
```

## 校验

```powershell
Get-FileHash -Algorithm SHA256 .\data\S17\S17-battle-reports-20261109.ywrlib.json
node -e "const fs=require('fs');const j=JSON.parse(fs.readFileSync('data/S17/S17-battle-reports-20261109.ywrlib.json','utf8'));console.log(j.reports.length)"
```

## 许可与声明

本仓库中由维护者整理的数据结构、提示词和文档按 [CC BY 4.0](LICENSE) 发布。游戏名称、角色名称、战法名称和其他相关内容的权利归各自权利人所有；本仓库与游戏发行方无隶属或授权关系。

请自行确认数据使用符合适用法律、平台规则和相关权利人的要求。提交新数据前，请移除截图、账号、联系方式、设备标识、访问凭据和其他个人或敏感信息。
