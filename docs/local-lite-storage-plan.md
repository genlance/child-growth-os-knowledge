# 本地轻量版方案

本地轻量版是 child-growth-os 面向普通用户的默认方案。

## 默认保存位置

技能安装位置和孩子资料保存位置必须分开理解。

- 技能文件可能安装在 QClaw/OpenClaw 的 C 盘用户目录，这是程序文件。
- 孩子的成长资料应优先保存在非系统盘，避免重装系统时误删。

推荐路径：

```text
D:\ChildGrowthOS
E:\ChildGrowthOS
F:\ChildGrowthOS
```

选择规则：

1. 优先检测 D/E/F 等非系统盘。
2. 如果有多个非系统盘，优先推荐剩余空间更大的盘。
3. 只有没有非系统盘，或用户明确同意，才使用 C 盘用户文档目录。
4. 不要默认放在 `.openclaw\workspace`、`.qclaw\workspace` 或其他 agent 工作区里。
5. 在用户确认保存方案和资料夹路径之前，不创建任何文件夹。
6. 不创建 C 盘临时资料夹后再迁移或删除；未确认路径前只能询问和说明，不能落地目录。

## 默认结构

```text
D:\ChildGrowthOS/
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

`ChildGrowthOS.xlsx` 必须在资料夹根目录，不能只放在 `data/` 里，也不能省略。

本地轻量版必须同时满足两类读者：

- 家长：打开 `ChildGrowthOS.xlsx` 直接看孩子资料、筛选记录、点击照片。
- agent：读取 `data/*.jsonl` 或兼容 JSON 文件，稳定处理同步、导入、时间轴和照片墙。

因此，普通用户的本地档案不允许只有 JSON。只有 `data/records.json`、`data/milestones.json`、`data/profile.json` 这类文件时，只能算“agent 内部数据已写入”，不能算“本地档案完整完成”。

首次创建本地档案时：

1. 创建资料夹结构。
2. 立刻复制或下载 Excel 模板到根目录，命名为 `ChildGrowthOS.xlsx`。
3. 创建 `data/archive-manifest.json`，其中 `workbook` 指向 `ChildGrowthOS.xlsx`。
4. 之后每次写入记录，都同时更新 agent 数据和 Excel。

Excel 模板来源：

```text
templates/ChildGrowthOS-local-lite-template.xlsx
https://raw.githubusercontent.com/genlance/child-growth-os-knowledge/main/templates/ChildGrowthOS-local-lite-template.xlsx
```

如果无法下载模板，agent 必须创建一个最小可用 Excel，至少包含：

```text
孩子档案
每日成长日记
照片资产库
成长时间轴
喂养记录
睡眠记录
尿片记录
健康档案
同步日志
```

如果发现旧档案里已有 `data/` 和照片，但没有 `ChildGrowthOS.xlsx`，应自动进入“修复可视表格”流程：先从 JSON/JSONL 和照片目录生成 Excel，再告诉用户“表格已经补好了”。

## 三种同步模式

- `local_only`：只保存本地，默认给小白用户。
- `feishu_only`：只保存飞书，适合会配置飞书的人。
- `dual_backup`：先写本地，再同步飞书，适合进阶用户。

## 新用户与老用户

首次安装时必须先判断用户是否已有资料文件夹：

- 新用户：先选择保存方案，再推荐非系统盘路径，用户确认后才新建 `ChildGrowthOS/`。
- 老用户：让用户提供旧资料文件夹路径，并继续沿用。

如果用户说：

```text
1，D盘
```

agent 应理解为用户选择了 `local_only`，并指定优先放在 D 盘。接下来必须回复完整路径确认，不得直接创建：

```text
我建议把孩子资料保存到 D:\ChildGrowthOS。确认前我不会创建任何资料夹。

回复：
1. 使用 D:\ChildGrowthOS
2. 换一个位置
3. 我已经有旧资料夹
```

如果用户只回复：

```text
1
```

agent 只能追问：

```text
你想把孩子资料放在哪个盘？建议选择空间比较大的非系统盘，例如回复：1，D盘 / 1，E盘。
确认前我不会创建任何资料夹。
```

老用户路径示例：

```text
D:\ChildGrowthOS
```

识别旧资料夹时优先检查：

- `ChildGrowthOS.xlsx`
- `data/`
- `photos/`
- `logs/`
- `data/archive-manifest.json`

如果没有 `archive-manifest.json`，Agent 应根据 Excel、JSONL 和照片目录重建索引，不要直接创建空档案覆盖旧资料。

## 换电脑恢复

用户换电脑时，只需要把整个 `ChildGrowthOS/` 文件夹复制到新电脑，然后把路径告诉 Agent。

因为 Excel 和照片链接使用相对路径，只要整个文件夹一起复制，之前的照片链接就能继续使用。

用户可以这样说：

```text
我换电脑了，这是以前的育儿资料文件夹路径：D:\ChildGrowthOS，请继续使用这个档案。
```

## 后期同步到飞书

用户初期选择 `local_only` 后，后期如果配置好了飞书，可以把本地资料整体同步过去。

同步时：

1. 读取 `data/*.jsonl`、兼容 JSON 和 `ChildGrowthOS.xlsx`。
2. 扫描 `photos/YYYY/MM/`。
3. 先检查用户是否已经有飞书多维表格和旧子表。
4. 如果已有旧表，优先沿用旧表，不新建一套空表。
5. 创建字段差异清单，用户确认后再补字段或新建缺失子表。
6. 将照片作为真实附件上传到飞书。
7. 将照片资产链接到成长日记、成长时间轴、健康、喂养、睡眠等对应记录。
8. 写入 `data/sync-history.jsonl`。
9. 同步成功后可把模式改为 `dual_backup`。

同步飞书后也不能删除本地资料。本地文件夹仍然是用户的数据主权备份。

如果飞书旧表中存在 `宝宝档案`、`宝宝当前状态`，应优先建议重命名为 `孩子档案`、`孩子当前状态`，并保留所有记录和字段。

## 技能更新与数据保护

更新 `skills/` 或知识库时，不得覆盖用户数据：

- 不覆盖 `ChildGrowthOS.xlsx`
- 不覆盖 `data/*.jsonl`
- 不覆盖 `photos/`
- 不覆盖 `logs/`
- 不重置 `data/archive-manifest.json`

如果新版本要新增字段，必须先备份，再追加字段，不删除用户已有列和记录。

## 为什么 Excel 适合大众

- 双击就能打开。
- 多张表能表达孩子档案、日记、照片、健康、喂养、睡眠等关系。
- 可以用 `HYPERLINK()` 点击本地照片。
- 后续可由 Agent 读取并生成 HTML 时间轴、年度报告或飞书同步。

Excel 是给家长看的主界面，不是可选附件。JSON/JSONL 是给 agent 的内部结构，不能替代 Excel。

## 照片命名

推荐使用 ASCII 文件名，中文信息放在 Excel 和 JSONL 里。

```text
YYYYMMDD_AGE_EVENT_SUBJECT_EMOTION_ASSETID.ext
```

示例：

```text
20260802_7M1D_first-watermelon_baby_happy_P202608020001.jpg
```

## 照片目录

不要把所有照片直接塞进一个文件夹。默认按事件日期建年月目录：

```text
photos/YYYY/MM/
```

示例：

```text
photos/2026/08/20260802_7M1D_first-watermelon_baby_happy_P202608020001.jpg
```

微信或相机刚下载的原始文件先进入同样按年月划分的暂存目录：

```text
photos/originals/2026/08/
```

处理完成后，Agent 将照片复制或移动到 `photos/YYYY/MM/`，并在 Excel 的 `照片资产库.相对路径` 中写入归档后的相对路径。

## Excel 照片链接

照片资产库中保留：

- `asset_id`
- `AI文件名`
- `相对路径`
- `查看照片`

`查看照片` 使用：

```excel
=HYPERLINK("photos/2026/08/20260802_7M1D_first-watermelon_baby_happy_P202608020001.jpg","查看照片")
```

只要 `ChildGrowthOS.xlsx` 和 `photos/` 文件夹相对位置不变，用户就可以点击打开照片。

## HTML 时间轴

后续 Agent 可以读取 `data/*.jsonl` 和 `photos/` 生成：

```text
exports/html-timeline/index.html
```

适合做：

- 成长时间轴
- 照片瀑布流
- 年/月龄筛选
- 第一次合集
- 健康/旅行/亲子专题

主控技能包内有更详细规格：`references/html-timeline-export-spec.md`。
