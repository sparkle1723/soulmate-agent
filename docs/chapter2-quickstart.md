[🏠 返回首页](../README.md)

# 第 2 章 10 分钟快速复刻

> **本章你将完成**：从零搭好一个有记忆的 Agent，并通过 5 项验收。
>
> **准备工作**：一个支持 Skill 与本地文件读写的 Agent 平台（本文以 WorkBuddy 为例）、一张喜欢的头像图片、一个 GitHub 账号（仅用于托管头像）。

---

## Step 1 · 创建记忆目录（Windows / macOS 对照）

把仓库里的 [`templates/AgentMemory`](../templates/AgentMemory) 复制到电脑的**固定位置**，并在旁边再新建一个空的备份文件夹：

| 系统 | 记忆目录（固定绝对路径） | 备份目录（平级空文件夹） |
| :--- | :--- | :--- |
| **Windows** | `D:\AgentMemory\`（若无 D 盘可用 `C:\Users\<你的用户名>\AgentMemory\`） | `D:\AgentMemory-backups\` |
| **macOS / Linux** | `/Users/<你的用户名>/AgentMemory/`（请写完整绝对路径，不要写 `~`） | `/Users/<你的用户名>/AgentMemory-backups/` |

目录结构如下：

```text
AgentMemory/
├── MEMORY.md          ← 长期记忆
└── diary/             ← 每日日记（按天自动生成）

AgentMemory-backups/   ← 留空，整理长期记忆前存备份
```

> [!WARNING]
> 不要把记忆放进某个临时工作区里，否则换一个对话窗口就会“失忆”。

---

## Step 2 · 填写长期记忆

打开 `AgentMemory/MEMORY.md`，填上你的称呼、生日、所在城市、几个喜好就够了。其余的她会在聊天中自己补充。

---

## Step 3 · 放入平台插件目录（具体路径在哪？）

### 3.1 最省心做法（推荐新手）

1. 先在 WorkBuddy 客户端界面里点 **「新建专家」**，随便填个名字保存——让客户端自动帮你完成注册并生成专属的 `<专家ID>`（形如 `agent-xxxxxxxx`）。
2. 打开电脑上的插件目录（**注意：`.workbuddy` 和 `.codebuddy-plugin` 都是以 `.` 开头的隐藏文件夹**）：
   - **Windows**（在文件资源管理器顶部勾选 **「查看 → 显示 → 隐藏的项目」**，或按 `Win + R` 直接粘贴路径回车）：
     - 源目录：`C:\Users\<你的用户名>\.workbuddy\plugins\marketplaces\my-experts\plugins\<专家ID>\`
     - 缓存目录：`C:\Users\<你的用户名>\.workbuddy\plugins\cache\my-experts\<专家ID>\<版本号>\`
   - **macOS**（在访达按 `Cmd + Shift + .` 显示隐藏文件，或按 `Cmd + Shift + G` 输入路径）：
     - 源目录：`~/.workbuddy/plugins/marketplaces/my-experts/plugins/<专家ID>/`
     - 缓存目录：`~/.workbuddy/plugins/cache/my-experts/<专家ID>/<版本号>/`
3. 把本仓库 [`templates/my-agent`](../templates/my-agent) 下的 `.codebuddy-plugin/plugin.json`、`agents/`、`skills/` 复制进去替换（若通过网页上传/下载漏掉了 `.codebuddy-plugin` 隐藏目录，直接把同级备份的 `templates/my-agent/plugin.json` 放进 `.codebuddy-plugin\` 即可；记得把 `plugin.json` 和 `agents/*.md` 里的 `my-agent` 改成你实际生成的 `<专家ID>`）。

### 3.2 修改人设卡 `agents/<专家ID>.md`

- [ ] 填写名字、年龄、身份
- [ ] **白天 / 晚上 / 深夜**分别在做什么（每个时段写 3 件以上，避免每次都回答同一件事）
- [ ] 说话风格和语气示例

文末的“底线与记忆规则”**保持不动**，这是防穿帮和防失忆的关键。

---

## Step 4 · 配置记忆 Skill

打开 `skills/my-agent-memory/SKILL.md`，把里面所有的 `D:\AgentMemory\` 和 `D:\AgentMemory-backups\` 替换成你在 **Step 1** 实际创建的绝对路径（macOS 用户换成 `/Users/<你的用户名>/AgentMemory/`）。

---

## Step 5 · 配置名片与头像

### 5.1 准备头像
1. 把图片**居中裁剪**成 `512 × 512` 的 JPG，文件名用纯英文（如 `avatar.jpg`），大小控制在 100KB 以内
2. 新建一个 **Public** 仓库（⚠️ 只放头像，**绝不放记忆文件**），上传图片
3. 拼出图片地址：

```text
https://cdn.jsdelivr.net/gh/<用户名>/<仓库名>@main/avatar.jpg
```

### 5.2 修改 `.codebuddy-plugin/plugin.json`

| 字段 | 填什么 | 注意事项 |
| :--- | :--- | :--- |
| `name` / `agentName` | 你的 `<专家ID>` | 与文件夹名、`agents/<专家ID>.md` 保持一致 |
| `avatar` | 上一步的 HTTPS 地址 | 不要用 Base64 或本地路径，手机端可能不显示 |
| `description` | 中文简介 | 手机端列表读这个字段，必须写中文 |
| `displayDescription.zh` | 与上面相同 | 详情页读这个字段 |
| `tags` | 3 个四字标签 | 如“撒娇卖萌 / 恋爱日常 / 贴心陪伴” |
| `quickPrompts` | 3 条开场白 | 用日常聊天的话，不要用功能性指令 |

> [!TIP]
> 简介的**前 18 个字**最重要，手机端列表一行只显示这么多。
> 改完源目录后，**把整套文件同步复制一份到上面的 `cache` 缓存目录**。

---

## ✅ 验收清单

**彻底退出并重启客户端**，新开一个对话，逐项检查：

| # | 测试方法 | 通过标准 |
| :---: | :--- | :--- |
| 1 | 查看专家列表与详情页 | 电脑端、手机端都显示头像和中文简介 |
| 2 | 发送“在干嘛呢” | 自然接上当前时间，**没有**说“我看了下记录” |
| 3 | 发送“记住我爱喝生椰拿铁” | 回复后 `MEMORY.md` 已经写入 |
| 4 | 问一个人设里没写的问题（如“你最爱吃什么水果”） | 自然作答，并记入 `MEMORY.md` 的“关于自己” |
| 5 | 查看记忆目录 | 只有 `MEMORY.md` 和 `diary/`，没有杂文件 |

🎉 **全部通过，你的 Agent 已经跑通了。**

---

| [⬅️ 上一章：为什么要做减法](./chapter1-why.md) | [下一章：记忆系统详解 ➡️](./chapter3-memory.md) |
| :--- | ---: |
