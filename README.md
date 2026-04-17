<p align="center">
  <a href="https://feynman.is">
    <img src="https://github.com/getcompanion-ai/feynman/blob/main/assets/hero.png" alt="Feynman CLI" width="800" />
  </a>
</p>
<p align="center">开源 AI 研究代理</p>
<p align="center">
  <a href="https://feynman.is/docs"><img alt="文档" src="https://img.shields.io/badge/docs-feynman.is-0d9668?style=flat-square" /></a>
  <a href="https://github.com/getcompanion-ai/feynman/blob/main/LICENSE"><img alt="许可证" src="https://img.shields.io/github/license/getcompanion-ai/feynman?style=flat-square" /></a>
</p>

---

### 安装

**macOS / Linux：**

```bash
curl -fsSL https://feynman.is/install | bash
```

**Windows (PowerShell)：**

```powershell
irm https://feynman.is/install.ps1 | iex
```

单行安装脚本会获取最新的标记版本。如需固定版本，请显式传递版本号，例如 `curl -fsSL https://feynman.is/install | bash -s -- 0.2.30`。

安装程序会下载独立的原生包，包含其自带的 Node.js 运行时。

后续如需升级独立应用，请重新运行安装程序。`feynman update` 仅刷新 Feynman 环境内已安装的 Pi 包，不会替换独立的运行时包本身。

如需卸载独立应用，请删除启动器和运行时包，然后可选择性地删除 `~/.feynman` 以同时删除设置、会话和已安装的包状态。如果还想删除 alphaXiv 登录状态，请删除 `~/.ahub`。平台特定的路径请参阅安装指南。

本地模型通过设置流程支持。对于 LM Studio，运行 `feynman setup`，选择 `LM Studio`，并保持默认的 `http://localhost:1234/v1`，除非您更改了服务器端口。对于 LiteLLM，选择 `LiteLLM Proxy` 并保持默认的 `http://localhost:4000/v1`。对于 Ollama 或 vLLM，选择 `Custom provider (baseUrl + API key)`，使用 `openai-completions`，并指向本地的 `/v1` 端点。

### 仅安装技能

如果您只想安装研究技能，而不需要完整的终端应用：

**macOS / Linux：**

```bash
curl -fsSL https://feynman.is/install-skills | bash
```

**Windows (PowerShell)：**

```powershell
irm https://feynman.is/install-skills.ps1 | iex
```

这会将技能库安装到 `~/.codex/skills/feynman`。

如需在仓库本地安装：

**macOS / Linux：**

```bash
curl -fsSL https://feynman.is/install-skills | bash -s -- --repo
```

**Windows (PowerShell)：**

```powershell
& ([scriptblock]::Create((irm https://feynman.is/install-skills.ps1))) -Scope Repo
```

这会安装到当前仓库下的 `.agents/skills/feynman`。

这些安装程序会下载捆绑的 `skills/` 和 `prompts/` 目录树，以及这些技能引用的仓库指导文件。它们不会安装 Feynman 终端、捆绑的 Node 运行时、认证存储或 Pi 包。

---

### 你输入的 → 会发生什么

```
$ feynman "what do we know about scaling laws"
→ 搜索论文和网络资源，生成带引用的研究简报

$ feynman deepresearch "mechanistic interpretability"
→ 多代理并行调研，综合分析和验证

$ feynman lit "RLHF alternatives"
→ 文献综述，包含共识、分歧和开放问题

$ feynman audit 2401.12345
→ 对比论文声明与公开代码库

$ feynman replicate "chain-of-thought improves math"
→ 在本地或云端 GPU 上复现实验
```

---

### 工作流

自然语言输入或使用斜杠命令作为快捷方式。

| 命令 | 功能 |
| --- | --- |
| `/deepresearch <topic>` | 深度多源调研 |
| `/lit <topic>` | 论文搜索和原始文献综述 |
| `/review <artifact>` | 模拟同行评审，含严重程度和修订计划 |
| `/audit <item>` | 论文与代码库不匹配审计 |
| `/replicate <paper>` | 在本地或云端 GPU 复现实验 |
| `/compare <topic>` | 来源对比矩阵 |
| `/draft <topic>` | 基于研究发现撰写论文式草稿 |
| `/autoresearch <idea>` | 自主实验循环 |
| `/watch <topic>` | 持续研究监控 |
| `/outputs` | 浏览所有研究成果 |

---

### 代理

四个捆绑的研究代理，自动调度。

- **Researcher** — 跨论文、网页、仓库、文档收集证据
- **Reviewer** — 模拟同行评审，分级反馈
- **Writer** — 基于研究笔记撰写结构化草稿
- **Verifier** — 内联引用、源 URL 验证、死链清理

---

### 技能与工具

- **[AlphaXiv](https://www.alphaxiv.org/)** — 论文搜索、问答、代码阅读、批注（通过 `alpha` CLI）
- **Docker** — 隔离容器执行，确保本地实验安全
- **Web 搜索** — Gemini 或 Perplexity，零配置默认启用
- **会话搜索** — 跨研究会话的索引回忆
- **预览** — 浏览器和 PDF 导出生成的成果
- **Modal** — 无服务器 GPU 计算，用于突发训练和推理
- **RunPod** — 持久 GPU 容器，支持 SSH 访问，用于长期运行实验

---

### 工作原理

基于 [Pi](https://github.com/badlogic/pi-mono) 构建代理运行时，使用 [alphaXiv](https://www.alphaxiv.org/) 进行论文搜索和分析，并通过 CLI 工具实现计算和执行。功能以 [Pi 技能](https://github.com/badlogic/pi-skills) 形式交付 —— Markdown 指令文件在启动时同步到 `~/.feynman/agent/skills/`。每个输出都有来源支撑 —— 声明链接到论文、文档或仓库的直接 URL。

---

### Star 历史

<a href="https://www.star-history.com/?repos=getcompanion-ai%2Ffeynman&type=date&legend=top-left">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/chart?repos=getcompanion-ai/feynman&type=date&theme=dark&legend=top-left" />
    <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/chart?repos=getcompanion-ai/feynman&type=date&legend=top-left" />
    <img alt="Star 历史图表" src="https://api.star-history.com/chart?repos=getcompanion-ai/feynman&type=date&legend=top-left" />
  </picture>
</a>

---

### 参与贡献

查看 [CONTRIBUTING.md](https://github.com/getcompanion-ai/feynman/blob/main/CONTRIBUTING.md) 获取完整的贡献者指南。

```bash
git clone https://github.com/getcompanion-ai/feynman.git
cd feynman
nvm use || nvm install
npm install
npm test
npm run typecheck
npm run build
```

[文档](https://feynman.is/docs) · [MIT 许可证](https://github.com/getcompanion-ai/feynman/blob/main/LICENSE)

---

> 本项目汉化自 [https://github.com/getcompanion-ai/feynman](https://github.com/getcompanion-ai/feynman)
