# dsh-novel-hit

给 [DeepSeek Harness](https://deepseek.com) 用的写作插件。装进 Harness 后，它的技能目录里会多出 7 个写长篇网文的技能，写新书、改开篇、搭大纲、写章末、诊断数据时按需调用。

插件本身不生成文字，也不承诺写出爆款。它提供的是可执行的方法：判断标准、模板、检查表，以及会被反复用到的边界。

## 注册的技能

| 技能 | 什么时候用 |
|---|---|
| `baokuan-novel` | 开新书、定题材与卖点、写书名简介、判断一本书能不能起来 |
| `novel-golden-three` | 写前三章、改开篇、开篇数据差 |
| `novel-outline` | 搭大纲、列细纲、分卷、写到一半发现线乱了 |
| `novel-hooks` | 写章末钩子、改断章位置、追读率上不去 |
| `novel-character` | 设计主角配角反派、人物立不住、读者说人物崩了 |
| `novel-dialogue-scene` | 写对话与场景、稿子被判「水」「干」「像说明文」 |
| `novel-diagnose` | 贴数据、掉收藏、评论变差、判断还有没有救 |

每个技能都包含：适用条件、可直接套的模板或表格、自检清单、常见错误。内容用中文写，面向起点、番茄、七猫、晋江、知乎盐选等平台的常见写法，平台指标只做定性描述，不给编造的阈值。

## 安装

需要 DeepSeek Harness 与已配置的 profile（桌面版或 Web 版均可）。

1. 把本仓库克隆到本地，或下载 release 包解压。
2. 在 Harness 里让 agent 用 `plugin_manager` 安装这个目录，`action: install_bundle`，`target` 传目录的绝对路径。
3. 重启 Harness，让 Host 重新加载 bundle。

安装后 `skills/` 下的每个 `.md` 都会被注册进技能目录。插件没有配置项，不需要填参数。

## 用法

装好后直接说需求即可，例如：

- 「我想写一本女频年代文，主角是下岗女工，帮我定卖点和前三章结构。」
- 「这是第一章，第一屏没冲突，按黄金三章改一版。」
- 「追读在 20 章开始掉，这是 20 到 30 章的章节卡，帮我做逐章复盘。」

agent 会加载对应技能，按里面的模板和检查表执行。

## 目录结构

```
dsh-novel-hit/
├── index.js            # Host apply()：读取 skills/*.md 并注册
├── cordis.patch.yml    # bundle 补丁，插入插件行
├── skills/*.md         # 7 个技能，frontmatter + markdown 正文
├── locale/zh.json      # 插件显示名与描述（中文）
├── locale/en.json      # 插件显示名与描述（英文）
├── icon.svg            # 插件图标
├── NOTICE.md
└── LICENSE
```

## 工作方式

Host 在激活时列出 `skills/*.md`，解析文件头部的 frontmatter（`name`、`description`、`whenToUse`、`license`），把正文通过 `ctx.skills.register()` 注册成运行时技能。每次注册都包在 `ctx.effect()` 里，禁用或卸载插件时会依次注销。

技能正文存在磁盘上，目录里只显示摘要，不用时不占上下文。

## 自定义

- **删技能**：删掉或改名 `skills/<name>.md`（改成 `.md.disabled` 之类的后缀）。
- **加技能**：放一个新的 `skills/<kebab-case>.md`，frontmatter 保持同样的字段。
- 插件行支持 `config.disabled`（字符串数组），列在里面的技能名不注册。
- 改完要重启 Harness，Host 会重新读目录。

## 边界

- 不提供抄改、洗稿、套用他人设定与桥段的写法。
- 不编造平台数据、推荐机制、读者画像。不确定的地方写「不确定」。
- 数据诊断基于形状和相对变化，不给「必须达到多少」的绝对阈值。

## 许可

插件代码与技能文本：MIT，见 `LICENSE`。引用与来源说明见 `NOTICE.md`。
