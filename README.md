# Memory Manager — OpenClaw Skill

结构化长期记忆管理系统，让 OpenClaw Agent 拥有可靠的长期记忆。

## ✨ 核心功能

- **自动捕获** — 7 种触发条件自动写入记忆（纠正、偏好、决策...）
- **分层存储** — 永久偏好 / 业务经验 / 活跃任务 / 对话摘要
- **自动瘦身** — 5KB 上限，智能优先级删除
- **纠正追踪** — 自动检测 6 种纠正信号，晋升为永久规则
- **隐私隔离** — 群聊/私聊严格分离
- **精准检索** — 每次 session 自动加载 + 按需 memory_search

## 📦 安装

```bash
# 方式 1: ClawhHub (推荐)
clawhub install memory-manager

# 方式 2: 手动
# 下载 .skill 文件放入 workspace/skills/ 解压
```

## 🏗️ 记忆架构

```
MEMORY.md (≤5KB)          ← 核心记忆
├── 🔒 用户偏好（永久）
├── 💼 角色定位（永久）
├── 📝 业务经验（≤15条）
├── 📋 活跃任务（完成删）
├── 🔄 纠正记录（≤10条）
└── 💬 对话摘要（7天精简）

memory/YYYY-MM-DD.md      ← 每日日志
memory/archive/            ← 归档
.learnings/corrections.md  ← 纠正详细记录
```

## 🤝 协作

与以下 Skill 互补：
- **Self-Improving / Proactive Self-Improving** — 错误/学习捕获
- **Lossless-Claw (LCM)** — 对话级持久化
- **High Agency** — 态度保障

## 📄 License

MIT
