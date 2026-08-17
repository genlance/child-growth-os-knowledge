# 子技能分发协议

本协议用于 child-growth-os 主控技能从 GitHub 仓库发现、推荐、安装和更新子技能。

知识拉取 raw 地址、每日推送、月龄概览和用户修改推送时间规则见：

```text
docs/knowledge-update-and-push-schedule.md
```

## 分层

```text
首次入口：`skills/child-growth-os/`，QClaw / WorkBuddy 初次安装，用于温暖陪伴、微信式记录、照片归档、每日成长日记、21点复盘和轻量知识推送。
GitHub Hub：child-growth-os-knowledge，存放知识、版本、子技能 manifest。
知识分析技能：`huiba-parenting-os`，当用户需要系统分析、书籍内化、观点评估或知识卡时再推荐。
子技能分为两层：基础技能公开到 GitHub，可直接安装；高级技能只在 GitHub 展示技能名字、适用阶段、能力介绍和使用示例，不公开实际代码技能包。高级技能包、完整使用教程及后续更新统一收录于「晖爸育儿 Pro 技能库」。
微信触达：通知用户有新知识、新技能或技能更新。
```

## 子技能分层

基础技能：

- 可公开上传到 GitHub。
- `skills-manifest.json` 中可以提供 `path`、`entry`、`version` 和权限说明。
- 用户确认后，agent 可以按公开路径下载和安装。
- 适合作为信任入口、基础记录、导入导出和系统体验。

高级技能：

- GitHub 上只公开技能名称、适用年龄、解决的问题、能力介绍和示例成果。
- 不在公开 `skills-manifest.json` 中提供真实 `path`、`entry`、压缩包地址或 raw 安装地址。
- `installable` 必须为 `false`，`access_level` 使用 `premium`，`distribution_mode` 使用 `contact_author_pro`。
- 当用户想安装高级技能时，agent 不能假装下载或安装，应说明公开仓库仅展示介绍，并引导用户查看「晖爸育儿 Pro 技能库」获取方式。
- 用户获得 ZIP 技能包后，可以把 ZIP 发给 agent 安装。
- 统一联系口径：

```text
这个属于晖爸高级育儿子技能，公开仓库只展示介绍，不提供完整技能包。
高级技能包、完整使用教程及后续更新统一收录于「晖爸育儿 Pro 技能库」。
如需了解 Pro Skills，请在公众号 / 小红书 / 抖音搜索：晖爸育儿代码。
```

建议分层：

- 基础公开：`child-growth-os`、`archive-operator`。
- 高级 Pro：`huiba-parenting-os`、`english`、`montessori`、`blueprint`，以及后续主题课、能力规划、学习系统类子技能。

## Pro 技能库获取流程

推荐用户流程：

```text
GitHub 公开 Community 基础技能
    ↓
用户体验 Community 能力
    ↓
README / 微信推荐展示 Pro Skills
    ↓
用户通过公众号 / 小红书 / 抖音了解 Pro Skills
    ↓
进入「晖爸育儿 Pro 技能库」
    ↓
在飞书知识库下载 ZIP 技能包、查看教程和更新日志
    ↓
把 ZIP 发给 Agent
    ↓
Agent 执行安装并检查是否成功
```

面向小白用户的安装话术：

```text
把这个 ZIP 发给你的 Agent，然后说：
“帮我安装这个技能包，并检查是否安装成功。”
```

公开说明原则：

- GitHub 负责 Community 基础能力、公开文档和版本说明。
- Pro 技能库负责收录高级技能包、完整教程、更新日志和使用案例。
- Agent 负责帮助用户安装已获得的技能包。
- 公开文档保持用户视角，重点说明功能、适用场景、安装方式、数据安全和版本更新。

## 首次安装口径

对客户不要先推荐“知识操作系统”。第一句话应该推荐「晖爸育儿系统」Community 主控助手：

```text
请安装晖爸育儿系统主控助手：
https://github.com/genlance/child-growth-os-knowledge/tree/main/skills/child-growth-os
```

安装后的第一体验应该是：

- 欢迎新手爸妈。
- 先判断新用户还是老用户：新用户新建资料夹，老用户提供旧资料夹路径并继续沿用。
- 引导用户选择备份同步方案：简单本地保存、飞书同步、双备份。
- 简单询问家长希望怎么称呼、孩子昵称、生日/月龄。
- 鼓励用户直接发一句成长记录或照片。
- 新手先选择保存方案和资料夹路径，确认后才创建本地轻量档案。
- 每天做温和育儿知识推送和21点日记复盘。
- 只有在用户需要时才推荐知识分析、蒙氏、英语、成长蓝图等子技能。

注意：QClaw 等平台的“安装成功”页面可能只是展示技能简介，不会自动触发首次引导对话。因此安装成功后，应提示用户继续发送启动指令：

```text
开始使用晖爸育儿系统
```

新用户也可以直接发送：

```text
1，D盘
```

用户也可以回复 `1，E盘` 或 `1，F盘`。如果只回复 `1`，agent 应继续询问盘符，不得先在 C 盘或 agent workspace 创建临时资料夹。

微信/ClawBot 不作为第一步门槛。推荐顺序是：

```text
安装技能 -> 发送启动指令 -> 新建或识别资料夹 -> 选择本地/飞书/双备份 -> 再连接微信 ClawBot
```

首次引导完整协议见：

```text
docs/onboarding-and-data-continuity.md
```

## 自动与确认边界

可以自动：

- 检查 `version.json`。
- 检查 `releases/latest.json`。
- 拉取知识卡片、月龄索引、敏感期索引。
- 在飞书记录“发现了新版本/新技能”。

自动检查时应优先读取 raw 地址：

```text
https://raw.githubusercontent.com/genlance/child-growth-os-knowledge/main/version.json
https://raw.githubusercontent.com/genlance/child-growth-os-knowledge/main/releases/latest.json
https://raw.githubusercontent.com/genlance/child-growth-os-knowledge/main/knowledge-manifest.json
https://raw.githubusercontent.com/genlance/child-growth-os-knowledge/main/skills-manifest.json
```

必须用户确认：

- 安装新技能。
- 升级已安装技能。
- 启用会新增表字段、自动任务或外部权限的能力。
- 迁移旧数据结构。
- 从本地档案批量同步到飞书。

## 用户数据保护边界

安装或更新技能时，默认只更新技能和知识文件，不触碰用户资料文件夹。

禁止覆盖：

- `ChildGrowthOS.xlsx`
- `data/*.jsonl`
- `photos/`
- `logs/`
- `data/archive-manifest.json`

如果用户已经有旧资料文件夹，新版本技能必须继续沿用该文件夹。

如果用户换电脑，只需要复制整个 `ChildGrowthOS/` 文件夹，并告诉 agent 新路径。

## 微信推荐模板

```text
我发现「家庭英语启蒙规划与管理教练」已经适合小晖当前阶段。

它可以帮你：
1. 制定家庭英语输入计划
2. 选择适合月龄/年龄的绘本和音频
3. 把英语启蒙记录写回 child-growth-os

是否安装？
回复：安装英语启蒙
```

## 安装流程

1. 客户先安装 `skills/child-growth-os/`。
2. 主控助手读取 `skills-manifest.json`。
3. 根据宝宝月龄、当前状态、用户问题匹配可推荐技能。
4. 微信发送推荐，等待用户确认。
5. 如果是基础公开技能，用户确认后下载对应 `skills/{id}/`。
6. 如果是高级 Pro 技能，不得下载或安装；发送 Pro Skills 了解方式，并写入 `已安装子技能` 为 `待获取Pro技能`。
7. 基础公开技能安装到目标平台的技能目录或按平台要求导入。
8. 写入本地/飞书 `已安装子技能` 记录。
9. 发送安装完成提示和示例用法；高级 Pro 技能只发送获取方式和适用说明。

## 更新流程

1. 每月或用户手动触发时读取 raw 版本的 `releases/latest.json`。
2. 对比本地/飞书记录里的已安装技能版本。
3. 如果有新版，先展示 changelog。
4. 用户确认后再更新。
5. 更新后写入版本记录。

## 状态表建议

在飞书新增 `已安装子技能` 表：

- 技能ID
- 技能名称
- 当前版本
- 技能层级
- 分发方式
- 是否可公开安装
- 获取方式
- 推荐年龄
- 安装状态
- 安装时间
- 上次检查时间
- GitHub路径
- 权限说明
- 用户确认记录
- 备注
