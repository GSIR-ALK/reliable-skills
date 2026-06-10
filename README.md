# 🛠 Reliable Skills

精选可靠 Skill 清单，按场景分类。后续引入新 Agent 时，可批量安装这些经过实战验证的技能。

---

## 📋 技能索引

### 一、创意 & 内容生成

| 项目 | GitHub 地址 | 简介 | 推荐理由与用途 |
|------|------------|------|--------------|
| **excalidraw** | https://github.com/excalidraw/excalidraw | 手绘风格白板/架构图/流程图工具 | 画系统架构图、流程图、时序图，输出 JSON 格式可二次编辑，比 PlantUML 更直观。适合做方案演示和文档配图 |
| **ascii-art** | - (Hermes 内置) | pyfiglet + cowsay + boxes + img2ascii | 快速生成字符画标题、终端艺术字、图片转字符画，适合黑客风演示 |
| **p5js** | https://p5js.org/ | 生成艺术/交互式可视化/3D 场景 JavaScript 库 | 做交互式数据可视化、粒子特效、数学动画，单个 HTML 即可运行，无需构建工具 |

### 二、开发 & 代码

| 项目 | GitHub 地址 | 简介 | 推荐理由与用途 |
|------|------------|------|--------------|
| **writing-plans** | - (Hermes skill) | 出方案 → 确认 → 执行的完整工作流 | 任何涉及"先想清楚再做"的场景。自动产出方案文档，分步执行，避免跳过计划直接写代码 |
| **test-driven-development** | - (Hermes skill) | RED-GREEN-REFACTOR 三阶段 TDD 流程 | 需要确保代码质量时使用，先写测试再写实现，减少回归 bug |
| **systematic-debugging** | - (Hermes skill) | 4 阶段根因调试法：理解→诊断→修复→验证 | 遇到难复现或复杂的 bug 时使用，防止"修好了但不知道为什么好" |
| **github-pr-workflow** | - (Hermes skill) | GitHub PR 全生命周期管理 | 从分支创建到合并的完整流程，配合 code review 使用 |
| **github-code-review** | - (Hermes skill) | PR 差异审查 + 内联评论 | 需要审查他人代码或自审时使用，安全性检查 + 代码质量门禁 |
| **subagent-driven-development** | - (Hermes skill) | 子代理执行 + 双阶段审查 | 大型功能拆解，子代理并行开发不同模块，然后统一审查合并 |

### 三、产品 & 商业

| 项目 | GitHub 地址 | 简介 | 推荐理由与用途 |
|------|------------|------|--------------|
| **product-validation** | - (Hermes skill) | 从市场信号→竞品分析→技术方案→盈利模式→路线图的全链路验证 | 想做一个新项目但不确定值不值得做时使用，30 分钟到 2 小时出结论 |
| **validate-idea** | - (Hermes skill) | 轻量级想法验证：痛点/市场/技术/盈利 4 维度判断 | 比 product-validation 更轻量，15 分钟判断一个点子是否靠谱 |
| **app-inception** | - (Hermes skill) | 从原始想法经过市场验证→合规检查→产品定位→进入构建流程 | 适合"我有一个 App 想法但不知道从哪开始"的场景 |
| **free-apis** | - (Hermes skill) | 1564 条免费 API 索引 + 已测试可用的列表 | 做原型或 MVP 时不想花钱买 API，用这个找免费替代品 |

### 四、学习 & 知识

| 项目 | GitHub 地址 | 简介 | 推荐理由与用途 |
|------|------------|------|--------------|
| **cyber-learning** | - (Hermes skill) | 苏格拉底式 Web 安全引导教学，14 节课覆盖核心漏洞 | 零基础学网安，不直接给答案，通过提问引导自己推导出结论。每节课配乌云实战案例复盘 |
| **guided-learning** | - (Hermes skill) | 通用苏格拉底引导学习法 | 当用户问"这题怎么做"时，不直接给答案，通过追问帮用户自己发现答案 |
| **src-bugbounty-kb** | - (Hermes skill) | SRC 漏洞挖掘知识库（乌云案例 + PayloadsAllTheThings） | 学完网安基础后实战用，查阅真实漏洞案例和 Payload 库 |

### 五、运维 & 部署

| 项目 | GitHub 地址 | 简介 | 推荐理由与用途 |
|------|------------|------|--------------|
| **server-admin** | - (Hermes skill) | 阿里云 Linux 服务器运维手册 | 记录服务器配置、防火墙、Nginx、MySQL 状态、进程管理等，新人接手服务器不用重新摸索 |
| **qq-official-bot** | - (Hermes skill) | QQ 官方机器人 WebSocket/WebHook 接入 | 对接 QQ 频道的标准流程，包含密钥交换、WebSocket 生命周期、Ed25519 签名验证、Nginx 反代配置 |

### 六、AI 模型 & 推理

| 项目 | GitHub 地址 | 简介 | 推荐理由与用途 |
|------|------------|------|--------------|
| **llama-cpp** | https://github.com/ggml-org/llama.cpp | 本地运行 GGUF 格式 LLM 的 C++ 推理引擎 | 服务器无 GPU 也能跑小模型（如 Qwen2.5-1.5B），完全离线，零 API 成本 |
| **dashscope-image-generation** | - (Hermes skill) | 通义万相 API 图像生成 | 阿里云免费的 AI 画图接口，生成头像、封面、插图等，无需 GPU 服务器 |
| **vllm** | https://github.com/vllm-project/vllm | 高吞吐 LLM 推理框架 | 有 GPU 时的首选推理引擎，支持 PagedAttention 和连续批处理 |
| **obliteratus** | - (Hermes skill) | LLM 拒绝回答消融（diff-in-means 方法） | 需要模型不拒绝某些合理问题时使用，操作简单但威力大 |

---

## 🚀 一键安装脚本

新 Agent 接入时，执行以下命令即可安装所有 skill：

```bash
# 安装 npm/pip 工具
npm install -g @anthropic-ai/claude-code
pip3 install dashscope

# 克隆必要仓库
git clone https://github.com/excalidraw/excalidraw.git /tools/excalidraw

# Hermes skill 安装（Hermes 内置 skill 无需额外安装）
```

> 注：Hermes 内置 skill（如 writing-plans、cyber-learning 等）由 Hermes 自动加载，无需单独安装。此处列出的主要是 GitHub 开源项目和外置工具。

---

## 📝 贡献指南

新增可靠 skill 的标准：
1. ✅ 经过至少一次实际项目验证
2. ✅ 有明确的用途场景说明
3. ✅ 安装/配置方式清晰
4. ✅ 不是临时造轮子

提交格式：
```
| **项目名** | GitHub 地址 | 一句话简介 | 为什么推荐 + 什么场景用 |
```
