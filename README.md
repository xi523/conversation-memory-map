# Conversation Memory Map｜对话记忆地图

[中文](README.md) · [English](README_EN.md)

把容易淹没在线性上下文里的长对话，转成两张可以持续维护的地图：一张追踪当前讨论状态，一张沉淀长期可复用知识。

![知识库共建对话思考地图示例](examples/knowledge-base-conversation-map.png)

## 为什么做这个 Skill

长对话的问题通常不是“内容不够”，而是结构逐渐消失：聊过什么、为什么做出某个判断、哪些问题还没解决，都会被后续消息淹没。

这个 Skill 不把聊天记录重新摘要一遍，而是维护两种互补的记忆：

| 地图 | 作用 | 主要内容 |
|---|---|---|
| 对话思考地图 | 工作记忆：看清现在聊到哪里 | 讨论分支、状态、理由、当前结论、下一步 |
| 主题知识地图 | 长期记忆：沉淀以后还能复用的知识 | 概念、稳定结论、适用边界、证据、开放问题、版本 |

两张地图通过一条简单规则连接：**讨论 → 确认 → 判断是否可复用 → 沉淀为知识**。未经确认的观点不会被包装成事实，临时沟通也不会污染长期知识。

## 安装

将仓库克隆到 Codex Skills 目录：

```bash
git clone https://github.com/xi523/conversation-memory-map.git ~/.codex/skills/conversation-memory-map
```

目录已经存在时，不要覆盖，请先检查已安装的版本。重新打开 Codex 任务后即可显式调用。

## 如何触发

这个 Skill 依赖人工触发，已关闭隐式调用，不会在后台自行修改地图。首次使用或进入新任务时，用下方的 `$conversation-memory-map` 显式调用；同一对话中已经约定此工作流后，可以简称：

> 同步对话地图

也可以显式调用并指定范围：

```text
$conversation-memory-map 同步当前对话：更新思考地图，并把本轮已经确认且可复用的结论沉淀到主题知识地图，在右侧打开思考地图。
```

只更新当前讨论状态：

```text
$conversation-memory-map 只更新对话思考地图，不更新主题知识地图。
```

只做知识沉淀：

```text
$conversation-memory-map 检查本轮哪些内容已经形成稳定结论，去重后同步到主题知识地图，并保留依据、适用边界和版本变化。
```

## 对话思考地图长什么样

它不是普通的知识分类图，而是一张从左到右的讨论状态图。根节点表示总命题，只展开当前焦点所在的分支；节点固定使用“状态、短标题、一行说明”三段式，完整理由和下一步通过点击查看。

状态颜色保持固定：绿色表示已确认，红色表示正在讨论，紫色表示后面再展开，灰色表示暂时放下。颜色不再承担主题分类，避免越画越乱。

## 更新原则

每次同步优先更新已有节点的状态、结论和下一步，只有出现新的独立问题才增加分支。同一主题的地图原位更新，不为每轮对话重复创建文件或标签页。

当讨论节点转为“已确认”时，Skill 会判断它是否具有未来复用价值。只有稳定、可追溯、能说明适用边界的内容，才会进入主题知识地图。

## 项目结构

```text
conversation-memory-map/
├── SKILL.md                       # Codex 使用的英文 Skill 指令
├── SKILL.zh-CN.md                 # 中文版 Skill 指令
├── agents/openai.yaml             # Skill 展示与调用配置
├── references/map-spec.md         # 英文双地图规范
├── references/map-spec.zh-CN.md   # 中文双地图规范
└── examples/
    └── knowledge-base-conversation-map.png
```

## License

[MIT](LICENSE)
