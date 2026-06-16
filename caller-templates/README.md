# Caller workflow stubs

Drop these into a **consuming repo** at `.github/workflows/` to use the shared
bots hosted in `repo-template` — no workflow logic copied, just a thin caller.

## Use
1. Copy the three stubs (`pr-review.yml`, `issue-response.yml`, `mention-response.yml`)
   into the consuming repo's `.github/workflows/`.
2. Config resolution (the reusable workflow reads `vars.LLM_MODEL` / `vars.LLM_BASE_URL`
   / `secrets.LLM_API_KEY` in the **caller's** context):
   - **Public repo** → inherits **org-level** `LLM_*` automatically (free plan OK).
   - **Private repo** → set them at **repo level** (org config can't reach private repos on free plan).
3. `secrets: inherit` passes the caller's secrets (org or repo) into the reusable workflow.

Pin is `@v1`. Prompts are fetched from `repo-template@master` at runtime.
