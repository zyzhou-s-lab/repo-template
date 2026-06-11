# 新仓库启用清单

基于本模板 **Use this template** 建出新仓库后，按下面四步配置。模板复制了所有文件，但 **不会复制 secrets、variables、仓库设置**——这些必须每个新仓库重配。

## 1. 配置 Secret（必需）

`Settings → Secrets and variables → Actions → Secrets`：

| Secret | 用途 | 必需 |
|--------|------|------|
| `LLM_API_KEY` | 三个 bot 调用 LLM 端点的 key | ✅ 是（不配 bot 全部失败） |

> `GITHUB_TOKEN` 是 Actions 自动注入的，无需手动配。

## 2. 配置 Variables

`Settings → Secrets and variables → Actions → Variables`：

| Variable | 用途 | 必需 | 示例 |
|----------|------|------|------|
| `LLM_BASE_URL` | Anthropic 兼容端点 | ✅ 是 | MiMo：`https://token-plan-cn.xiaomimimo.com/anthropic`；官方 Anthropic：`https://api.anthropic.com` |
| `LLM_MODEL` | 模型名（同时映射 opus/sonnet/haiku 别名） | ✅ 是 | `mimo-v2.5-pro` / `claude-sonnet-4-6` / … |
| `BOT_LOGINS` | 被视为"本 bot"的账号登录名（逗号分隔），用于去重/防自触发 | 否，默认 `github-actions[bot]` | — |

> **本模板是 provider 中立的**：换模型 / 换厂商只需改 `LLM_BASE_URL` + `LLM_MODEL` + `LLM_API_KEY` 三个值，不动 workflow 文件。
> 三者**没有内置默认**——`LLM_BASE_URL` / `LLM_MODEL` 不填，bot 会跑错或报错，这是刻意为之（避免静默指向意料之外的模型）。
>
> `BOT_LOGINS`：只有当 bot 评论是以非 `github-actions[bot]` 身份（例如某个 PAT/App）发出时，才需要把那个登录名加进去，否则去重失效会导致重复评论。

## 3. 打开仓库开关（必需）

`Settings → Actions → General → Workflow permissions`：

- ☑ **Read and write permissions**
- ☑ **Allow GitHub Actions to create and approve pull requests**

> 这两项是 **release-please 自动开 Release PR** 和 **mention bot 提 PR** 的前提。不开，release-please 静默不开 PR。

## 4. 替换占位符（必需）

全仓搜索并替换：

| 占位符 | 替换成 | 出现位置 |
|--------|--------|----------|
| `{{PROJECT_NAME}}` | 你的项目名 | 三个 `.github/prompts/*.md` 顶部 |
| `<one-line description ...>` 那段 + `**Structure:**` 列表 | 真实的项目简介和目录结构 | 三个 prompt 的 `## Project Context` |
| `my-project` | 包名 | `pyproject.toml`、`release-please-config.json` |
| `example_pkg` | 你的真实包名 | `pyproject.toml`、`src/` 目录名 |

可选：
- bot 署名 marker `*Repo Bot*` 想改名的话，**workflow 里的 `const marker` 和 prompt 里的署名必须同步改**（去重逻辑靠它字符串匹配）。
- 召唤词 `@zyzhou`：在 `mention-response.yml` 的 `if:` 和 `mention-response.md` 里。想换召唤词两处都要改。
- `tests.yml`：默认只跑 Python `pytest`；有前端就把底部 `frontend` job 取消注释并改 `working-directory`。
- 默认分支：`tests.yml` 和 `release-please.yml` 都已同时监听 `master` 和 `main`，无需改。

## 验证

- 新建一个 issue → 几分钟内应出现一条以 `*Repo Bot*` 结尾的中文回复。
- 提一个 PR → 应触发自动 review。
- 往默认分支合入一个 `feat:` / `fix:` commit → release-please 应开出一个 "chore(main): release x.y.z" 的 Release PR。
