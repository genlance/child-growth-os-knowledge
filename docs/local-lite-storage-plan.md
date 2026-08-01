# 本地轻量版方案

本地轻量版是 Baby Growth OS 面向普通用户的默认方案。

## 默认结构

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

## 三种同步模式

- `local_only`：只保存本地，默认给小白用户。
- `feishu_only`：只保存飞书，适合会配置飞书的人。
- `dual_backup`：先写本地，再同步飞书，适合进阶用户。

## 为什么 Excel 适合大众

- 双击就能打开。
- 多张表能表达宝宝档案、日记、照片、健康、喂养、睡眠等关系。
- 可以用 `HYPERLINK()` 点击本地照片。
- 后续可由 Agent 读取并生成 HTML 时间轴、年度报告或飞书同步。

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
