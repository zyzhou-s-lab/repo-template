# repo-template

可复用的仓库脚手架模板。包含三个 agentic GitHub bot、release-please 自动 CHANGELOG/发版、pytest CI。

> 这是一个 **Template repository**。在 GitHub 上点 **Use this template**（或 `gh repo create <new> --template zyzhou-saffron/repo-template`）即可基于它新建仓库。

## 内含什么

| 文件 | 作用 |
|------|------|
| `.github/workflows/issue-response.yml` + `prompts/issue-response.md` | 新 issue 自动回复（只读，问答式） |
| `.github/workflows/mention-response.yml` + `prompts/mention-response.md` | `@zyzhou` 召唤 bot：可答疑、可建分支提 PR |
| `.github/workflows/pr-review.yml` + `prompts/pr-review.md` | PR 自动 review（含 follow-up 增量 review） |
| `.github/workflows/release-please.yml` + `release-please-config.json` + `.release-please-manifest.json` | 自动维护 `CHANGELOG.md` + 升 `pyproject.toml` 版本 + 打 tag 发版 |
| `.github/workflows/tests.yml` | PR / push 上跑 `pytest`（前端 job 默认注释掉） |
| `pyproject.toml` / `src/example_pkg/` / `tests/` | 最小可跑的 Python 占位骨架 |

bot 由 `anthropics/claude-code-base-action` 驱动，**provider 中立**：通过 `LLM_BASE_URL` / `LLM_MODEL` / `LLM_API_KEY` 三个仓库级配置指向任意 Anthropic 兼容端点（MiMo、官方 Anthropic、自建网关…），换模型不用动 workflow。

## 基于本模板建新仓库后，必做清单

见 **[SETUP.md](./SETUP.md)** —— 列出了必须配置的 secrets/variables、必须打开的仓库开关，以及需要替换的占位符。**不配这些 bot / 发版流程跑不起来。**
