# 三国：谋定天下演武大会 S16 战报库

本仓库公开一份《三国：谋定天下》演武大会 S16 结构化战报数据，以及将战报截图整理为统一文本记录时使用的 AI 提示词。

## 数据集

- 文件：[`s16-battle-reports-20260812.ywrlib.json`](https://github.com/CharlesWang505/yanwu-s16-battle-reports/releases/download/s16-20260812/s16-battle-reports-20260812.ywrlib.json)
- 格式：`yanwu-report-library` v1
- 赛季：S16
- 战报数量：4,769
- 导出时间：`2026-08-12T10:42:28.089Z`
- SHA-256：`6a39f1402f4609518b8c90ad9a09f8e78b379208917c94702afd1632a96a7156`

每条记录保留对阵双方的阵型、武将、战法、剩余兵力和战果等结构化字段。数据不包含原始截图、账号信息、联系方式、访问令牌、服务器配置或用户数据。

## 使用提示词

批量识别战报截图时，使用 [prompts/battle-report-ocr.md](prompts/battle-report-ocr.md) 中的提示词。该提示词限定单批最多 20 张图片，并要求逐行输出固定结构，便于后续导入与校验。

## 数据结构

顶层对象：

```json
{
  "format": "yanwu-report-library",
  "version": 1,
  "exportedAt": "2026-08-12T10:42:28.089Z",
  "reports": []
}
```

`reports` 内每条记录使用以下主要字段：

```json
{
  "id": "UUID",
  "season": "S16",
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

`leftAnalysis`、`rightAnalysis` 等字段为原始导出中的阵容分析结果；使用者可按需忽略，只消费 `parsed` 字段。

## 校验

```powershell
Get-FileHash -Algorithm SHA256 .\s16-battle-reports-20260812.ywrlib.json
node -e "const fs=require('fs');const j=JSON.parse(fs.readFileSync('s16-battle-reports-20260812.ywrlib.json','utf8'));console.log(j.reports.length)"
```

预期 SHA-256 为上文所列值，战报数量为 `4769`。

## 许可与声明

本仓库中由维护者整理的数据结构、提示词和文档按 [CC BY 4.0](LICENSE) 发布。游戏名称、角色名称、战法名称和其他相关内容的权利归各自权利人所有；本仓库与游戏发行方无隶属或授权关系。

请自行确认数据使用符合适用法律、平台规则和相关权利人的要求。提交新数据前，请移除截图、账号、联系方式、设备标识、访问凭据和其他个人或敏感信息。
