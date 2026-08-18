# 三国：谋定天下演武大会 S7-S16 战报库

[繁體中文](README.zh-TW.md)

本仓库公开《三国：谋定天下》演武大会 S7-S16 的结构化战报数据，以及将战报截图整理为统一文本记录时使用的 AI 提示词。

## 数据集

数据按赛季独立存放在 [`data/`](data/) 目录中。文件来自网站公开接口，已去除原始 OCR 文本、截图、用户提交信息、账号信息、访问凭据和服务端配置。
部分战报来源：https://github.com/liamqma/sanmou-yanwu/tree/master/data/battles

| 赛季 | 战报数量 | 文件 |
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

格式为 `yanwu-report-library-public` v1。每条记录保留战果、左右阵型、武将、战法和剩余兵力等结构化字段。

S1-S6 不包含在本公开战报数据集中；S1-S2 的经典推荐资料不属于本仓库的主战报库。

## 使用提示词

批量识别战报截图时，使用 [prompts/battle-report-ocr.md](prompts/battle-report-ocr.md) 中的提示词。提示词限定单批最多 20 张图片，并要求逐行输出固定结构，便于后续导入和校验。

## 数据结构

顶层对象示例：

```json
{
  "format": "yanwu-report-library-public",
  "version": 1,
  "season": "S16",
  "exportedAt": "2026-08-18T00:00:00.000Z",
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

## 校验

```powershell
Get-FileHash -Algorithm SHA256 .\data\S16\S16-battle-reports-20260818.ywrlib.json
node -e "const fs=require('fs');const j=JSON.parse(fs.readFileSync('data/S16/S16-battle-reports-20260818.ywrlib.json','utf8'));console.log(j.reports.length)"
```

## 许可与声明

本仓库中由维护者整理的数据结构、提示词和文档按 [CC BY 4.0](LICENSE) 发布。游戏名称、角色名称、战法名称和其他相关内容的权利归各自权利人所有；本仓库与游戏发行方无隶属或授权关系。

请自行确认数据使用符合适用法律、平台规则和相关权利人的要求。提交新数据前，请移除截图、账号、联系方式、设备标识、访问凭据和其他个人或敏感信息。
