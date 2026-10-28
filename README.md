# 英文词汇表 · English Vocabulary Lists

剑桥 KET、PET 与托福（TOEFL）三个等级的英语词汇表，已整理为可直接程序读取的结构化数据。

每个等级提供 **CSV**（便于 Excel 查看、人工校对）与 **JSON**（便于程序加载）两种格式，字段完全一致。

## 词表一览

| 等级 | 考试 | CEFR | 词条数 | CSV | JSON |
|---|---|---|---|---|---|
| KET | Cambridge A2 Key | A2 | 1,723 | [CSV](ket/KET单词表_A2Key_2025版_1723词.csv) | [JSON](ket/KET单词表_A2Key_2025版_1723词.json) |
| PET | Cambridge B1 Preliminary | B1 | 3,099 | [CSV](pet/PET单词表_B1Preliminary_2025版_3099词.csv) | [JSON](pet/PET单词表_B1Preliminary_2025版_3099词.json) |
| TOEFL | 托福 | B2–C1 | 5,072 | [CSV](toefl/TOEFL单词表_5072词.csv) | [JSON](toefl/TOEFL单词表_5072词.json) |

总计 **9,894** 条词条，六个字段全部齐全，无空缺。

## 字段说明

CSV 表头：

| 列 | 字段 | 说明 | 示例 |
|---|---|---|---|
| 1 | `#` | 序号 | `1` |
| 2 | 单词 | 词条，含词组与多拼写变体 | `a/an` |
| 3 | 音标 | 英式 IPA，斜杠包裹 | `/ə/, /ən/` |
| 4 | 中文含义 | 词性用中文简写前缀，多义项用 `；` 分隔 | `限. 一个、一件` |
| 5 | 英文例句 | 完整句，以句号收尾 | `I would like a cup of tea, please.` |
| 6 | 例句中文 | 例句的直译 | `请给我一杯茶。` |
| 7 | 词性 | 原始词性标记 | `det` |

JSON 为对象数组，字段名对应如下：

```json
{
  "id": "ket-0001",
  "word": "a/an",
  "pos": "det",
  "ipa": "/ə/, /ən/",
  "zh": "限. 一个、一件",
  "example": "I would like a cup of tea, please.",
  "example_zh": "请给我一杯茶。"
}
```

说明几点：

- **词条 ID 稳定不变**：KET 为 `ket-0001`…`ket-1723`，PET 为 `pet-0001`…`pet-3099`，TOEFL 为连续序号。词表修订时 ID 不会重排，适合作为用户学习记录的外键。
- **中文含义的词性前缀**用中文简写（名 / 动 / 形 / 副 / 介 / 连 / 代 / 限 / 叹 / 助 / 数 / abbr / phr），而非英文缩写，便于直接展示。
- **音标为英式（RP）**，与剑桥官方词汇表口径一致。

## 数据来源

### KET / PET

词条、词性、官方例句来自剑桥大学英语考评部（Cambridge English）公开的官方词汇表 PDF：

- A2 Key Vocabulary List (August 2025)
- B1 Preliminary Vocabulary List (2025)

这两份词汇表由剑桥官方免费公开发布，供教师与学习者使用。

补充字段来源：

| 字段 | 来源 |
|---|---|
| 音标 | [ipa-dict](https://github.com/open-dict-data/ipa-dict)（en_UK）+ [ECDICT](https://github.com/skywind3000/ECDICT) 回退 |
| 中文释义 | ECDICT + 人工校对 |
| 例句（缺失时补充） | [Tatoeba](https://tatoeba.org/) 英语语料（CC BY 2.0 FR） |
| 例句中文 | 机器翻译 + 人工校对 |

PET 有 1,605 条词条与 KET 重合，这部分直接沿用了 KET 已校对过的音标与释义，未重复生成。

### TOEFL

词条与中文释义来自一份流传较广的 TOEFL 词表（Excel 格式，2004 年编制）。**该词表原始出处与授权状况不明**，无法追溯到具体出版物。使用时请自行评估合规性。

这份数据没有音标和例句，全部由以下来源补充：

| 字段 | 来源 | 数量 |
|---|---|---|
| 音标 | ipa-dict + ECDICT + 复合词拼合 + 屈折形式回退 + 人工 | 5,072 |
| 英文例句 | Tatoeba | 4,815 |
| 英文例句 | 模型针对该词补写（Tatoeba 无语料的低频词） | 250 |
| 英文例句 | 人工补写（原表特殊写法，如 `confuse...with...`） | 7 |
| 例句中文 | 机器翻译 | 5,072 |

整理过程中修正了原始数据的若干问题：合并 324 条重复词条、修正 11 处拼写错误（如 `propery` → `property`、`incorpoarate` → `incorporate`）、清理释义中的领域标记与非规范词性标记。

## 数据质量

三个词表的六个字段完整性：

```
KET     1,723 条   单词 0 · 词性 0 · 音标 0 · 释义 0 · 例句 0 · 例句中文 0
PET     3,099 条   单词 0 · 词性 0 · 音标 0 · 释义 0 · 例句 0 · 例句中文 0
TOEFL   5,072 条   单词 0 · 词性 0 · 音标 0 · 释义 0 · 例句 0 · 例句中文 0
```

数据经过机器校验（格式、完整性、重复、编码）与人工抽查，但**未逐条人工审校**。如需用于严肃出版或教学场景，建议自行抽样复核。

## 使用建议

```python
import json

with open('ket/KET单词表_A2Key_2025版_1723词.json', encoding='utf-8') as f:
    vocab = json.load(f)

for entry in vocab[:5]:
    print(entry['word'], entry['ipa'], entry['zh'])
```

CSV 为 UTF-8 with BOM 编码，Excel 直接双击打开不会乱码。

## 授权

本仓库的**整理成果**（数据清洗、字段补全、格式转换）可自由使用。

但请注意词条内容的来源差异：

- KET / PET 词条源自剑桥官方公开词汇表，原创性内容（官方例句等）版权归剑桥大学考评部所有
- TOEFL 词条原始出处不明，无法确认授权
- 补充的例句来自 Tatoeba（CC BY 2.0 FR），音标数据来自 ipa-dict 与 ECDICT（各自遵循其原始许可）

如需商业使用，建议就上述来源逐一确认授权。本仓库仅为技术整理，不构成任何授权主张。

## 更新记录

| 日期 | 内容 |
|---|---|
| 2026-10-28 | 首次发布：KET 1,723 · PET 3,099 · TOEFL 5,072 |
