# 旧版 Coze 育儿助手知识迁移清单

来源目录：

```text
E:/codex/tmp/yuer-assistant-skill-v1.7.0/育儿助手Skill/config/
```

说明：旧包中有大量 0-3 岁结构化育儿知识，适合迁移到 `child-growth-os-knowledge`，作为 0-36 个月基础知识层。部分 JSON 文件存在未转义中文引号等格式问题，迁移前需要清洗为合法 JSON。

## 推荐迁移目标结构

```text
knowledge/
├── domains/
│   ├── feeding.json
│   ├── sleep.json
│   ├── health.json
│   ├── safety.json
│   ├── daily-care.json
│   ├── behavior-emotion.json
│   ├── potty-training.json
│   ├── parent-support.json
│   ├── toys.json
│   └── clothing-size.json
├── development/
│   ├── milestones-00-36.json
│   └── montessori-periods-00-36.json
└── push-cards/
    ├── infant-00-12.json
    ├── toddler-12-36.json
    └── parent-support.json
```

## 文件迁移建议

| 旧文件 | 内容 | 新位置 | 优先级 | 处理方式 |
|---|---|---|---|---|
| `parenting_knowledge.json` | 按月龄 daily tips、weekly focus、常见问题 | `knowledge/push-cards/infant-00-12.json`、`knowledge/push-cards/toddler-12-36.json` | 高 | 拆成短推送卡片 |
| `milestone_database.json` | 0-36 个月运动、语言、认知、社交里程碑 | `knowledge/development/milestones-00-36.json` | 高 | 可直接作为发展索引 |
| `montessori_periods.json` | 0-36 个月蒙氏敏感期 | `knowledge/development/montessori-periods-00-36.json` | 已完成预览 | 已重新清洗为月龄观察索引 |
| `montessori_activities.json` | 按月龄敏感期活动 | `knowledge/domains/montessori.json`、`knowledge/push-cards/montessori-00-06.json` | 已完成预览 | 已拆为家庭实践原则、高频场景和微信短推送 |
| `feeding_guide.json` | 6-36 个月辅食食谱、添加原则 | `knowledge/domains/feeding.json` | 高 | 拆出食谱和过敏观察卡片 |
| `feeding_standards.json` | 0-36 个月喂养标准 | `knowledge/domains/feeding-standards.json` | 高 | 稳定参考，不频繁更新 |
| `sleep_guide.json` | 睡眠时长、清醒窗口、睡眠问题、环境 | `knowledge/domains/sleep.json` | 高 | 可生成睡眠推送和问答 |
| `health_care.json` | 发热、常见病、疫苗、用药、急救 | `knowledge/domains/health.json` | 高 | 需保留医疗安全边界 |
| `safety_guide.json` | 居家、汽车、户外、意外预防 | `knowledge/domains/safety.json` | 高 | 有格式问题，需清洗 |
| `daily_care.json` | 洗澡、换尿片、穿衣、护肤、口腔、指甲 | `knowledge/domains/daily-care.json` | 中 | 适合日常提醒 |
| `behavior_guide.json` | 发脾气、分离焦虑、情绪识别、规则、攻击等 | `knowledge/domains/behavior-emotion.json` | 中 | 有格式问题，需清洗 |
| `potty_training.json` | 如厕训练 | `knowledge/domains/potty-training.json` | 中 | 适合 18-48 个月推送 |
| `parent_support.json` | 产后支持、父母压力、家庭分工等 | `knowledge/domains/parent-support.json` | 中 | 适合父母侧推送 |
| `toy_recommendations.json` | 按月龄玩具推荐 | `knowledge/domains/toys.json` | 中 | 注意避免变成带货 |
| `clothing_sizes.json` | 衣服尺码、季节提醒 | `knowledge/domains/clothing-size.json` | 低 | 可作为工具型查询 |

## 迁移原则

1. 不整包原样塞入，先清洗、去重、拆层。
2. `domains/` 保存完整知识，`push-cards/` 保存微信短推送。
3. 医疗、用药、急救内容必须保留“不诊断、不替代医生”的安全边界。
4. 食谱、玩具、护理技巧等适合持续更新；里程碑、基础标准适合稳定版本。
5. 每条知识尽量补充：
   - age_range_months
   - tags
   - trigger_signals
   - record_suggestion
   - safety_level
   - source

## 第一批建议先迁移

第一批不要太贪，建议先做 5 个文件：

1. `milestone_database.json`
2. `montessori_periods.json`
3. `parenting_knowledge.json`
4. `feeding_guide.json`
5. `sleep_guide.json`

这 5 个能立刻补齐：

- 月龄推送
- 敏感期判断
- 里程碑识别
- 辅食/过敏观察
- 睡眠记录解读

## 与当前 Child Growth OS 的关系

旧 Coze 包主要覆盖 0-3 岁，适合做 `child-growth-os-knowledge` 的婴幼儿基础层。

当前新系统定位 0-18 岁，后续还需要新增：

- 3-6 岁：幼儿园、规则感、社交、情绪、自理、绘本、英语启蒙。
- 6-12 岁：小学学习、阅读、数学、项目制学习、AI 素养、运动习惯。
- 12-18 岁：青春期沟通、自主学习、身份认同、数字素养、生涯启蒙。
