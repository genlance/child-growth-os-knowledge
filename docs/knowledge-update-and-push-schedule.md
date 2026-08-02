# 知识拉取与推送节奏协议

本协议规定 child-growth-os 安装后如何从 GitHub 拉取公开知识、什么时候给家长推送育儿知识、如何展示晖爸 0-18 岁成长知识库更新，以及用户如何通过微信修改推送时间。

## GitHub 拉取地址

agent 读取知识文件时应优先使用 raw 地址，不使用网页地址解析内容。

仓库主页：

```text
https://github.com/genlance/child-growth-os-knowledge
```

主控技能安装页：

```text
https://github.com/genlance/child-growth-os-knowledge/tree/main/skills/child-growth-os
```

机器读取 raw 地址：

```text
https://raw.githubusercontent.com/genlance/child-growth-os-knowledge/main/version.json
https://raw.githubusercontent.com/genlance/child-growth-os-knowledge/main/releases/latest.json
https://raw.githubusercontent.com/genlance/child-growth-os-knowledge/main/skills-manifest.json
https://raw.githubusercontent.com/genlance/child-growth-os-knowledge/main/knowledge-manifest.json
```

按 manifest 继续读取其他文件时，也应把仓库路径转换为 raw 地址：

```text
https://raw.githubusercontent.com/genlance/child-growth-os-knowledge/main/{path}
```

例如：

```text
https://raw.githubusercontent.com/genlance/child-growth-os-knowledge/main/monthly/month-00-36-index.json
```

## 拉取节奏

首次安装后：

1. 立即读取 `version.json`。
2. 立即读取 `releases/latest.json`。
3. 立即读取 `knowledge-manifest.json`。
4. 只拉取首次运行需要的月龄索引、敏感期索引、推送卡片和主控技能说明。

日常检查：

- 每天 14:30-16:30 之间随机轻量检查一次 `version.json` 和 `releases/latest.json`。
- 如果版本没有变化，不拉取其他文件。
- 如果版本有变化，再按 `knowledge-manifest.json` 拉取需要更新的知识文件。

子技能更新：

- 每月或用户手动触发时检查 `skills-manifest.json` 和 `releases/latest.json`。
- 安装新技能、升级已安装技能、启用新增字段或自动任务时，必须先让用户确认。

## 用户可修改推送时间

默认推送时间可以被用户通过微信渠道修改。

用户可以这样说：

```text
以后每天育儿知识改成早上9点发。
```

```text
晚上复盘改成22点。
```

```text
月龄知识概览改成每个月当天晚上8点发。
```

agent 应更新本地或飞书中的推送设置，并回复确认：

```text
好的，已经改好。以后每日育儿知识会在 09:00 推送；今晚复盘仍然是 21:00。
```

如果用户只说“不想收到”，应询问是暂停每日知识、暂停月龄概览，还是全部暂停。

## 每日轻量育儿知识

默认时间：

```text
每天 11:30
```

内容原则：

- 只推 1 条。
- 不推大段知识。
- 不像广告。
- 尽量结合孩子当前月龄、近期记录和家庭场景。
- 给一个今天能观察或尝试的小动作。
- 底部可以轻轻标注来源，但不要每天放 GitHub 链接。

示例：

```text
早安，今天可以留意一个小细节：

孩子最近有没有反复练习同一个动作？
比如反复开合、搬东西、倒水、爬上爬下。

如果看到了，不用急着打断。
你可以拍一张照片或记一句话，我会帮你整理成成长观察。

来自：晖爸 0-18 岁成长知识库
```

## 每日成长日记复盘

默认时间：

```text
每天 21:00
```

内容原则：

- 复盘当天已记录内容。
- 问用户是否要补充一句话或照片。
- 如果用户补充，合并写入当天成长日记。

示例：

```text
今晚的小复盘来啦。
今天已经记录了：{events}，照片 {photo_count} 张。

还有什么想补充的吗？比如孩子当时的反应、谁在旁边、你自己的感受。你直接回一句话或发照片就行，我会补进今天的成长日记。
```

## 月龄育儿知识概览

默认时间：

```text
孩子出生日期每满一个月的当天 20:30
```

例如孩子出生于 2024-11-25：

- 2024-12-25 20:30 推送出生后第 1 个月概览。
- 2025-01-25 20:30 推送出生后第 2 个月概览。
- 2025-11-25 20:30 推送出生后第 12 个月概览。

如果某个月没有对应日期，例如出生于 31 号而当月没有 31 号，则使用当月最后一天 20:30。

内容定位：

- 不叫“GitHub 更新提醒”。
- 不像广告。
- 叫“本月育儿知识概览”或“第 X 个月成长重点”。
- 先说明今天是孩子出生后的第 X 个月。
- 概括这个月龄的核心育儿知识。
- 最后只放一句查看完整版口令。

模板：

```text
今天是{孩子称呼}出生后的第 {month_index} 个月。

这个月可以重点留意：

1. {月龄重点1}
2. {月龄重点2}
3. {月龄重点3}

我会结合你平时记录的喂养、睡眠、健康和成长瞬间，帮你把这些观察慢慢整理进孩子档案。

如果你想查看完整版，可以回复：晖爸育儿代码
```

`晖爸育儿代码` 的回复规则：

- 给出当月完整知识概览。
- 可以展示 GitHub 仓库页面链接。
- 仓库页面和 README 中应能看到公众号/自媒体名称：`晖爸育儿代码`。
- 不要强制用户安装新技能。

示例回复：

```text
这是晖爸 0-18 岁成长知识库的完整入口：

https://github.com/genlance/child-growth-os-knowledge

公众号/自媒体：晖爸育儿代码
```

## 推送边界

- 每日知识推送只给 1 条轻量提醒。
- 月龄概览每月只推一次，按孩子出生日期计算。
- GitHub 链接只在用户回复口令、查看来源、手动更新或安装技能时展示。
- 不把客户孩子照片、日记、飞书数据或本地资料上传到 GitHub。
- GitHub 只作为公开知识库和技能版本源，用户私有数据保存在用户自己的本地资料夹或用户自己的飞书。
