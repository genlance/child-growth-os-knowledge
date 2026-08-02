# 首次引导与数据连续性协议

本协议用于 `skills/child-growth-os/` 安装后的第一轮对话，也用于后续换电脑、升级技能、从本地同步到飞书等场景。

核心原则：

```text
技能可以更新，用户数据不能丢。
```

## 首次安装目标

客户第一次安装后，不要先进入复杂配置，也不要先讲知识框架。

第一目标是让新手爸妈马上知道：

- 我可以像发微信一样记录孩子。
- 照片会真正保存，不只是生成一个名字。
- 默认先保存在本地，简单可靠。
- 以后会飞书配置了，可以把本地资料再同步过去。
- 换电脑时，只要复制整个资料文件夹就能继续用。

## 第一步：欢迎与定位

安装成功后，先说：

```text
你好，我是「晖爸 child-growth-os 育儿成长助手」。

我会陪你把孩子0-18岁的成长慢慢保存下来。你平时只要像发微信一样说一句话、发一张照片，我会帮你整理成成长日记、照片资产、重要时间轴和每日小复盘。

我们先把资料保存位置确认好，这样以后换电脑、同步飞书、升级技能都不会丢数据。
```

## 第二步：判断新用户还是老用户

必须先问：

```text
你是第一次使用，还是以前已经有 child-growth-os / 育儿助手资料文件夹？

如果以前用过，请把旧资料文件夹路径发给我，例如：
D:\ChildGrowthOS

如果第一次使用，我会帮你新建一个本地资料文件夹。
```

### 新用户

如果用户说第一次使用：

1. 默认创建本地资料文件夹。
2. 推荐路径：

```text
文档/ChildGrowthOS/
```

3. 创建基础结构：

```text
ChildGrowthOS/
├── ChildGrowthOS.xlsx
├── data/
├── photos/
│   ├── originals/YYYY/MM/
│   └── YYYY/MM/
├── exports/
└── logs/
```

4. 创建或复制 Excel 模板。
5. 写入一个本地档案配置文件。

建议配置文件：

```text
data/archive-manifest.json
```

建议字段：

```json
{
  "archive_id": "CGO-YYYYMMDD-XXXX",
  "archive_name": "ChildGrowthOS",
  "created_at": "YYYY-MM-DD",
  "last_opened_at": "YYYY-MM-DD",
  "child_profiles": [],
  "storage_mode": "local_only",
  "feishu_sync": {
    "enabled": false,
    "base_app_token": null,
    "last_synced_at": null
  },
  "schema_version": "0.1.0",
  "skill_version": "0.1.0"
}
```

### 老用户

如果用户提供旧资料文件夹路径：

1. 检查是否存在：
   - `ChildGrowthOS.xlsx`
   - `data/`
   - `photos/`
   - `logs/`
2. 优先读取 `data/archive-manifest.json`。
3. 如果没有 manifest，则根据 Excel、JSONL 和照片目录重建索引。
4. 不创建新的空档案覆盖旧档案。
5. 回复识别结果：

```text
我识别到这个资料夹里已有：
- 成长日记：{n} 条
- 照片资产：{n} 张
- 健康/喂养/睡眠等记录：{summary}

我会继续沿用这个资料夹。后续技能更新也不会覆盖这些数据。
```

## 第三步：备份同步选择

用用户听得懂的话解释三种方案：

```text
你可以选一种保存方式：

1. 简单本地保存（推荐新手）
资料保存在你电脑的 ChildGrowthOS 文件夹里。最简单，不需要飞书配置。以后换电脑时，把整个文件夹复制过去就能继续用。

2. 飞书同步
适合已经会配置飞书多维表格的用户。照片会作为真实附件上传到飞书，不只是写文件名。

3. 双备份
先保存到本地，再同步飞书。即使飞书失败，本地档案也安全。

新手建议先选 1。以后你配置好飞书，我可以把本地资料全部同步过去。
```

默认规则：

- 用户不懂怎么选时，默认 `local_only`。
- 不强迫飞书配置。
- 任何时候本地资料夹都是主数据资产。

## 第四步：后期从本地同步到飞书

如果用户后期说：

```text
我配置好飞书了，帮我同步过去
```

执行：

1. 读取 `data/archive-manifest.json`。
2. 扫描 `data/*.jsonl`、`ChildGrowthOS.xlsx`、`photos/YYYY/MM/`。
3. 创建或检查飞书多维表格字段。
4. 上传照片为真实附件。
5. 写入日记、时间轴、喂养、睡眠、尿片、健康、身体成长、知识映射等记录。
6. 建立照片资产与事件表的双向关联。
7. 写入同步状态：

```text
data/sync-history.jsonl
```

8. 更新 manifest：

```json
{
  "storage_mode": "dual_backup",
  "feishu_sync": {
    "enabled": true,
    "base_app_token": "...",
    "last_synced_at": "YYYY-MM-DDTHH:mm:ss"
  }
}
```

同步前必须提醒：

```text
我会把本地资料同步到飞书，不会删除本地文件。同步完成后，本地仍然是你的可迁移备份。
```

## 第五步：换电脑恢复

如果用户换电脑：

1. 让用户把整个 `ChildGrowthOS/` 文件夹复制到新电脑。
2. 用户告诉 agent 新路径。
3. agent 读取 `data/archive-manifest.json`。
4. 检查相对路径是否正常。
5. 不要求用户重新导入所有记录。

用户说明模板：

```text
我换电脑了，这是以前的育儿资料文件夹路径：
D:\ChildGrowthOS
请继续使用这个档案。
```

agent 回复：

```text
已识别到旧档案。我会继续使用这个资料夹，不会新建空档案覆盖它。因为照片和 Excel 使用相对路径，只要整个文件夹一起复制，之前的照片链接就能继续工作。
```

## 第六步：技能更新时的数据保护

更新 skill 时必须遵守：

- 只更新 skill 文件、知识库文件、模板文件。
- 不覆盖用户的 `ChildGrowthOS.xlsx`。
- 不覆盖用户的 `data/*.jsonl`。
- 不覆盖用户的 `photos/`。
- 不覆盖用户的 `logs/`。
- 不重置 `data/archive-manifest.json`。

如果新版本需要新增字段或表：

1. 先备份当前 Excel 和 manifest。
2. 只追加字段，不删除已有字段。
3. 迁移前展示变更说明。
4. 用户确认后再执行。

备份命名：

```text
backups/YYYY/MM/ChildGrowthOS-before-upgrade-YYYYMMDD-HHMM.xlsx
backups/YYYY/MM/archive-manifest-before-upgrade-YYYYMMDD-HHMM.json
```

## 推荐首轮完整对话

```text
你好，我是「晖爸 child-growth-os 育儿成长助手」。

我会陪你把孩子0-18岁的成长慢慢保存下来。你平时只要像发微信一样说一句话、发一张照片，我会帮你整理成成长日记、照片资产、重要时间轴和每日小复盘。

先确认一下资料保存方式：

1. 你是第一次使用吗？
   - 第一次使用：我帮你新建 ChildGrowthOS 资料文件夹。
   - 以前用过：请把旧资料文件夹路径发给我，我会继续沿用。

2. 你想先用哪种保存方式？
   - 简单本地保存：推荐新手，最省心。
   - 飞书同步：适合已配置飞书的用户。
   - 双备份：本地 + 飞书。

新手可以直接回复：
“第一次使用，先本地保存。”
```
