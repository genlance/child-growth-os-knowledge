# 晖爸 0-18 岁成长知识库

> 每天说一句话，AI 帮你保存孩子 18 年的成长故事。

这是 **child-growth-os / 育儿AI成长系统** 的公开知识源，仓库名建议使用 `child-growth-os-knowledge`。它用于给 QClaw/OpenClaw/WorkBuddy 技能提供一个温暖的育儿成长助手，也提供持续更新的 0-18 岁育儿知识推送和按年龄阶段逐步启用的子技能。

## 内容范围

- 0-18 岁阶段性成长知识
- 温暖的新手爸妈育儿成长助手
- 微信式自然语言记录、照片归档和每日成长日记
- 0-36 个月月龄知识推送
- 蒙台梭利敏感期提醒
- 喂养、睡眠、尿片、健康、早教、亲子活动
- 幼儿园、小学、青春期前后的家庭成长议题
- 每周/每月育儿重点
- child-growth-os 系统更新说明
- 家庭英语启蒙、通才成长蓝图等可选子技能
- 晖爸育儿知识操作系统：四层地图、五层判断、CEO四层和知识卡标准
- 本地轻量版 Excel 模板、照片目录规范和 HTML 时间轴预留结构
- Agent 读取育儿资料、批量导入旧照片/旧日记、生成时间轴和专题照片墙
- 孩子成长数据表设计、飞书旧表延续升级和子表作用说明

## 首次安装

第一次建议安装的是主控助手，而不是知识分析技能：

```text
请安装晖爸 child-growth-os 育儿成长助手：
https://github.com/genlance/child-growth-os-knowledge/tree/main/skills/child-growth-os
```

安装后，用户可以直接像发微信一样记录：

```text
今天宝宝第一次吃西瓜，笑得很开心，还拍了两张照片。
```

主控助手会先做成长记录、照片归档、每日育儿日记和轻量知识推送。后续用户真的需要系统分析、蒙氏、英语启蒙或成长蓝图时，再推荐安装对应子技能。

## 使用方式

安装 child-growth-os 主控技能后，技能会定期读取本仓库的 `version.json` 和知识文件。

推荐推送节奏：

- 每日：月龄 + 敏感期 + 今日建议
- 每周：本周养育重点 + 适龄活动
- 每月：月龄发展报告 + 下个月关注点 + 仓库更新提醒

## 子技能机制

本仓库可以内置多个可选子技能。主控技能会根据宝宝年龄、近期记录和家长问题进行推荐，但不会静默安装。

当前预览子技能：

- `skills/child-growth-os/`：晖爸 child-growth-os 育儿成长助手，推荐第一个安装
- `skills/huiba-parenting-os/`：晖爸育儿知识操作系统，适合需要系统分析时再安装
- `skills/archive-operator/`：child-growth-os 育儿资料操作员，适合批量导入、时间轴、照片墙、迁移恢复和飞书同步时再安装
- `skills/english/`：家庭英语启蒙规划与管理教练
- `skills/blueprint/`：AI时代通才成长蓝图教练
- `skills/montessori/`：家庭蒙氏观察与环境教练

后续规划：

- `skills/reading/`：阅读与绘本成长教练
- `skills/adolescence/`：青春期家庭沟通教练

机器可读清单：

- `skills-manifest.json`
- `releases/latest.json`
- `knowledge-manifest.json`

安装原则：知识内容可以自动检查更新；安装或升级子技能必须经过用户确认。

## 知识操作系统

本仓库新增一个知识总控层，用来把零散育儿内容整理成可检索、可推送、可绑定到孩子真实记录的结构化知识。

```text
晖爸育儿知识操作系统 = 四层地图 + 五层判断 + CEO四层深挖
```

- 四层地图：`生理 / 安全 / 发展 / 文化`，用于给知识分类。
- 五层判断：`底线 / 节律 / 关系 / 能力 / 生态`，用于判断优先级和风险。
- CEO四层：`原理 / 模型 / 操作 / 经验`，用于把一个主题讲透并做成知识卡。

核心文件：

- `knowledge/frameworks/huiba-parenting-knowledge-os.json`
- `knowledge/maps/age-band-four-layer-map.json`
- `knowledge/schemas/knowledge-card.schema.json`
- `knowledge/schemas/viewpoint-evaluation.schema.json`

## 数据表设计

本系统面向 0-18 岁长期使用，核心表名统一为 `孩子档案`，不再使用只适合婴幼儿阶段的 `宝宝档案`。

数据表说明：

- `docs/child-growth-data-schema.md`

核心子表包括：

- 孩子档案
- 每日成长日记
- 照片资产库
- 成长时间轴
- 喂养记录
- 睡眠记录
- 尿片记录
- 健康档案
- 身体成长曲线
- 育儿知识映射
- 育儿实验闭环
- 孩子当前状态
- 家庭书信
- 已安装子技能
- 同步日志

如果用户已经有飞书旧表，agent 必须优先沿用旧表并生成字段差异清单，确认后再补字段。旧表 `宝宝档案`、`宝宝当前状态` 应作为兼容别名迁移为 `孩子档案`、`孩子当前状态`，不得重复新建空表覆盖旧数据。

## 本地轻量版

普通用户默认使用本地轻量版，不需要配置飞书：

```text
ChildGrowthOS/
├── ChildGrowthOS.xlsx
├── data/
├── photos/
│   ├── originals/
│   │   └── 2026/
│   │       └── 08/
│   └── 2026/
│       └── 08/
├── exports/
└── logs/
```

模板文件：

```text
templates/ChildGrowthOS-local-lite-template.xlsx
```

支持三种模式：

- `local_only`：只写本地。
- `feishu_only`：只写飞书。
- `dual_backup`：先写本地，再同步飞书。

照片必须按 `photos/YYYY/MM/` 归档，原始下载文件按 `photos/originals/YYYY/MM/` 暂存。本地 Excel 使用相对路径和 `HYPERLINK()` 打开照片，后续可以生成 HTML 成长时间轴。

## 资料操作与可视化

如果用户希望让 agent 调用已有育儿资料，生成成长时间轴、专题照片墙、年度报告，或把旧照片/旧日记批量导入，可以按需安装：

```text
请安装 child-growth-os 育儿资料操作员：
https://github.com/genlance/child-growth-os-knowledge/tree/main/skills/archive-operator
```

典型用法：

```text
这是我的育儿资料文件夹：D:\ChildGrowthOS。请读取里面的数据，帮我生成 2026 年成长时间轴网站。
```

```text
这是我以前整理的宝宝照片文件夹：D:\OldBabyPhotos。请先扫描，不要直接导入，给我一份待确认清单。
```

```text
帮我做一个“第一次合集”照片墙，只包含成长时间轴里标记为第一次的事件。
```

数据接口与导入导出协议：

- `docs/archive-operator-and-import-export.md`

## 维护者

**晖爸 / child-growth-os**

专注 AI 育儿系统、儿童成长数据资产、0-18岁育儿知识体系。

- GitHub: https://github.com/genlance/child-growth-os-knowledge
- 公众号/自媒体: 晖爸育儿代码
- 微信: huibayuerdaima

## 版权与医疗声明

本仓库内容仅供育儿记录、知识学习和家庭观察参考，不替代儿科医生诊断或治疗建议。涉及发热、过敏、呼吸困难、脱水、抽搐、外伤等情况，请及时咨询专业医生或就医。

使用或二次分发本知识库时，请保留来源署名和仓库链接。
