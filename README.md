# RimTalk - ET 思绪提示词（中文）

RimWorld 模组：把 [RimTalk - Expand Thoughts](https://steamcommunity.com/sharedfiles/filedetails/?id=3661175034)
里「对话结束后生成思绪」那次二次 LLM 调用的提示词换成**中文版**。

| 项 | 值 |
|---|---|
| packageId | `gnh.cn.cys.rimtalk.etprompt.cn` |
| 支持版本 | **RimWorld 1.6 / 1.7** |
| 依赖（前置） | RimTalk - Expand Thoughts（`zruic.expand.thoughts`）、RimTalk（`cj.rimtalk`） |
| 类型 | 纯 XML，**不含程序集** |
| 当前版本 | v1.0.1 |

## 它做什么

Expand Thoughts 在每段对话结束后会**再调一次 LLM**，让模型归纳出角色「心里形成的想法」。
那次调用用的是一段英文提示词。本模组**用同 defName 覆盖**它自带的 `ET_ThoughtSummarization`，
把 `systemPromptTemplate` 换成中文版，并补上几条更贴合角色扮演的规则：

- `text` 要写成角色**脑中的心里话**，不是总结报告，也不是他/她会说出口的话
- 想法允许比嘴上更真实、更强烈、更黑暗、更不体面
- 判断要建立在角色的**价值观**之上（该价值观由其文化信仰或人格天性定义）

## 安装

1. 先安装 **RimTalk** 与 **RimTalk - Expand Thoughts**（本模组的前置）
2. 把本仓库整个目录放进 RimWorld 的 `Mods\` 目录；或从 Releases 下载压缩包解压进去
3. 在游戏内模组列表里启用它。`About.xml` 已声明 `loadAfter`，游戏会自动把它排在 Expand Thoughts 之后

## 原理

Expand Thoughts 通过 `DefDatabase<ThoughtPromptDef>.GetNamed("ET_ThoughtSummarization")` 读取提示词。
RimWorld 的 `DefDatabase` **允许后加载的模组覆盖同名 def**（`AddAllInMods` 先 `Remove` 再 `Add`，后加载者胜），
因此本模组必须 `loadAfter` 排在它之后 —— 这一点写在 `About.xml` 里，由游戏保证。

## 注意事项

- 提示词在**首次使用时被静态缓存**，改动后需要**完全重启游戏**才生效
- RimTalk 提示词编辑器里的 "ET Thoughts Context" 是另一套东西（注入主对话的条目），与本模组无关
- 若 Expand Thoughts 将来改动该 defName，本模组会**自然失效（不报错）**
- 本模组带 `MayRequire="zruic.expand.thoughts"`：**前置缺失时整段跳过**，不会在日志里刷红字

## 变更记录

### v1.0.1 — 2026-10-16

- 加入 `MayRequire` 守卫：前置模组缺失时，由「刷一条 `Type … is not a Def type` 红字」改为**安静跳过**
- 修正 3 处文本：`or` → `或`；占位符后的多余空格；`"memory（某段记忆）"` 的引号错位
- 订正 aspect 规则误译：原文「同一个对象、同一个维度可以有互相矛盾的态度」→「同一个对象的不同维度各自独立共存」
- `supportedVersions` 补上 **1.7**（覆盖机制与游戏版本无关，1.6/1.7 通用）

### v1.0.0 — 2026-10-14

- 首次发布：`systemPromptTemplate` 全文中文化，并补两条规则（text 写成心里话；想法允许比嘴上更黑暗、更不体面）

## 许可

MIT License —— 见 [LICENSE](LICENSE)。预置提示词的著作权归其原作者所有。
