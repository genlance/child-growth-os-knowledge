# ChildGrowthOS 育儿资料操作与导入导出协议

本协议用于让 Codex、QClaw、OpenClaw、WorkBuddy 等 agent 读懂 ChildGrowthOS 本地档案结构，并在用户授权后执行批量导入、照片链接修复、HTML 时间轴、专题照片墙、年度报告和本地到飞书同步。

## 角色分工

- `child-growth-os`：温暖的育儿成长助手，负责微信式日常记录、照片归档、每日成长日记、轻量育儿推送和首次引导。
- `child-growth-archive-operator`：育儿资料操作员，负责识别资料结构、批量导入旧照片/旧日记、生成可视化作品、迁移恢复和同步飞书。
- `huiba-parenting-os`：育儿知识操作系统，负责把真实记录映射到四层地图、五层判断、CEO 四层和知识卡。

主控助手不应把首次体验变成技术配置。只有当用户提出“导入旧资料、做时间轴、照片墙、年度报告、换电脑恢复、同步飞书”等需求时，才推荐安装资料操作员。

## 标准资料夹

默认本地资料夹命名为 `ChildGrowthOS`。用户可自定义路径，但资料夹内部结构应保持稳定。

```text
ChildGrowthOS/
├── ChildGrowthOS.xlsx
├── data/
│   ├── archive-manifest.json
│   ├── baby-profile.json
│   ├── daily-journal.jsonl
│   ├── photo-assets.jsonl
│   ├── milestones.jsonl
│   ├── feeding.jsonl
│   ├── sleep.jsonl
│   ├── diaper.jsonl
│   ├── health.jsonl
│   ├── body-growth.jsonl
│   ├── knowledge-mapping.jsonl
│   ├── import-tasks.jsonl
│   └── sync-history.jsonl
├── photos/
│   ├── originals/
│   │   └── YYYY/
│   │       └── MM/
│   └── YYYY/
│       └── MM/
├── imports/
│   ├── raw/
│   ├── staging/
│   └── reports/
├── exports/
│   ├── html-timeline/
│   ├── photo-walls/
│   └── reports/
└── logs/
```

## 档案清单

`data/archive-manifest.json` 是 agent 识别资料夹的入口文件。

```json
{
  "archive_type": "ChildGrowthOS",
  "schema_version": "0.1.0",
  "created_at": "2026-08-02T21:00:00+08:00",
  "archive_id": "cgos_local_20260802_0001",
  "child_profile_file": "data/baby-profile.json",
  "workbook": "ChildGrowthOS.xlsx",
  "storage_mode": "local_only",
  "photo_root": "photos",
  "data_files": {
    "daily_journal": "data/daily-journal.jsonl",
    "photo_assets": "data/photo-assets.jsonl",
    "milestones": "data/milestones.jsonl",
    "health": "data/health.jsonl"
  }
}
```

agent 打开用户提供的资料夹时，应先查找 `data/archive-manifest.json`。如果不存在，但发现 `ChildGrowthOS.xlsx`、`data/`、`photos/` 等结构，应提示用户这是疑似旧档案，并询问是否补建 manifest。

## 核心数据

`daily-journal.jsonl` 每行是一条成长日记。

```json
{
  "record_id": "jrnl_20260802_210000_001",
  "date": "2026-08-02",
  "time": "21:00",
  "title": "第一次吃西瓜",
  "original_input": "今天第一次吃西瓜，笑得很开心。",
  "ai_story": "今天孩子第一次尝试西瓜，先小心地舔了一口，后来笑着继续要。",
  "age_months": 14,
  "tags": ["第一次", "辅食", "开心"],
  "linked_photo_asset_ids": ["photo_20260802_203012_001"],
  "linked_record_ids": ["mile_20260802_001"],
  "source_channel": "wechat",
  "confidence": 0.92
}
```

`photo-assets.jsonl` 每行是一张照片或一个附件资产。

```json
{
  "asset_id": "photo_20260802_203012_001",
  "date": "2026-08-02",
  "time": "20:30:12",
  "asset_type": "photo",
  "original_path": "photos/originals/2026/08/IMG_0012.jpg",
  "display_path": "photos/2026/08/2026-08-02_203012_第一次吃西瓜_001.jpg",
  "caption": "第一次吃西瓜时的笑脸",
  "tags": ["第一次", "辅食", "笑脸"],
  "linked_table": "daily_journal",
  "linked_record_ids": ["jrnl_20260802_210000_001"],
  "source_channel": "wechat",
  "import_task_id": null,
  "confidence": 0.9
}
```

其他 JSONL 文件都应通过 `linked_photo_asset_ids` 或 `linked_record_ids` 与照片资产库连接。健康档案、成长时间轴、喂养、睡眠、身体发育等记录不应单独散落保存图片。

## 批量导入

支持导入来源：

- 旧照片文件夹。
- 微信导出的聊天图片、视频、文本。
- Markdown、TXT、Word、Excel、CSV 格式的旧日记。
- 飞书多维表格导出内容。
- 其他宝宝相册或网盘整理资料。

导入流程：

1. 把原始资料复制或记录到 `imports/raw/`，不直接改动原文件。
2. 扫描文件，生成 `imports/staging/import-{timestamp}.jsonl`。
3. 根据 EXIF 时间优先判断照片日期；没有 EXIF 时使用文件修改时间、文件夹年月、文件名日期或用户提供日期。
4. 对低置信度项目生成确认清单，放到 `imports/reports/`。
5. 用户确认后，才写入 `data/*.jsonl`、`ChildGrowthOS.xlsx` 和 `photos/YYYY/MM/`。
6. 每次导入都写入 `data/import-tasks.jsonl`。

低置信度情况包括：日期不确定、同名重复、孩子身份不确定、图片内容疑似单据但无法识别类型、日记文本无法确定日期。

## 可视化导出

成长时间轴输出到：

```text
exports/html-timeline/index.html
```

时间轴应读取 `daily-journal.jsonl`、`milestones.jsonl` 和 `photo-assets.jsonl`，支持按年份、月份、标签、里程碑、照片数量过滤。所有图片链接使用相对路径，保证整个 `ChildGrowthOS` 文件夹复制到新电脑后仍可打开。

专题照片墙输出到：

```text
exports/photo-walls/{topic-slug}/index.html
```

常见专题：

- 第一次合集
- 笑脸合集
- 医院单据合集
- 旅行合集
- 亲子陪伴合集
- 蒙氏专注瞬间
- 生日成长合集

照片墙可按 `tags`、`event_type`、`date`、`people`、`place`、`linked_record_ids` 过滤。专题页必须保留原始记录入口，例如点击照片能看到关联的成长日记、健康记录或里程碑。

年度报告输出到：

```text
exports/reports/YYYY-growth-report.html
```

年度报告可以汇总身高体重、重要里程碑、高频标签、育儿知识映射、精选照片和父母留言。

## 飞书同步

本地轻量版用户后期配置飞书后，可整体同步到飞书。

同步规则：

- 同步前必须检查是否已有飞书 base 和旧子表。
- 如果已有旧表，优先沿用和补字段，不重复新建一套空表。
- `宝宝档案` 应作为旧名兼容，并优先迁移为 `孩子档案`。
- `宝宝当前状态` 应作为旧名兼容，并优先迁移为 `孩子当前状态`。
- 本地 `asset_id`、`record_id` 不得重写。
- 照片必须上传为飞书附件字段，不允许只写文件名或本地路径。
- 飞书子表应通过关联字段连接照片资产库。
- 同步完成后写入 `data/sync-history.jsonl`，记录本地 ID 与飞书记录 ID 的映射。
- 如果用户选择双备份，先写本地，再同步飞书；飞书失败时不影响本地记录。

## 给用户的常用指令

```text
这是我的育儿资料文件夹：D:\ChildGrowthOS。请读取里面的数据，帮我生成 2026 年的成长时间轴网站。
```

```text
这是我以前整理的宝宝照片文件夹：D:\OldBabyPhotos。请先扫描，不要直接导入，给我一份待确认清单。
```

```text
帮我做一个“第一次合集”照片墙，只包含成长时间轴里标记为第一次的事件。
```

```text
我已经配置好了飞书，请把本地 ChildGrowthOS 资料同步到飞书，照片要作为附件上传，并和对应事件关联。
```

## 安全原则

- 不删除用户原始照片、旧日记和旧表格。
- 不覆盖已有 `asset_id`、`record_id` 和 `archive-manifest.json`。
- 低置信度导入必须让用户确认。
- 所有导出作品使用相对路径。
- 所有导入、导出、同步任务都要写日志。
- 医疗单据只能归档和摘要，不替代医生诊断。
