# tasks/finalpool

Final pool of **validated, ready-to-run** tasks for `BenchTasksCollv3`.

## Current contents

The pool is **empty**. No task in the repository currently satisfies all
requirements, so nothing qualified for promotion.

## Why

Per `tasks/examples/example-task/readme.txt` and the framework
(`utils/data_structures/task_config.py`), a task is only runnable/valid if it
contains:

| Path | Required | Requirement |
| --- | --- | --- |
| `task_config.json` | **yes** | non-empty `needed_mcp_servers`; non-empty `needed_local_tools` containing `claim_done` |
| `docs/task.md` | **yes** | non-empty, English only |
| `docs/agent_system_prompt.md` | **yes** | non-empty, English only |
| `docs/user_system_prompt.md` | no | if non-empty, English only |
| `initial_workspace/`, `preprocess/main.py`, `groundtruth_workspace/`, `evaluation/main.py` | no | optional |

Every task added by the most recent commits on every developer branch ships
`docs/`, `evaluation/`, `preprocess/` and workspace folders, but **none of them
contains `task_config.json`**. Because `TaskConfig.build()` reads
`tasks/<split>/<task>/task_config.json` and indexes
`needed_mcp_servers` / `needed_local_tools` directly, these tasks raise
`FileNotFoundError`/`KeyError` and cannot be executed or evaluated.

## Latest-commit audit (29 new tasks, all `implementing`)

| Developer | Branch | Latest commit | New tasks | Status |
| --- | --- | --- | --- | --- |
| Fan | fan-dev | `8b90f345` | loyalty-program, discount-calculator | implementing |
| Gyy | gyy | `9612ffff` | tag-manager, sitemap-generator, robots-handler | implementing |
| Haoze | haoze | `e0072cf9` | media-organizer, streaming-service | implementing |
| Jl | jl_dev | `4e2b63f5` | customer-feedback-processor, inventory-management | implementing |
| Junteng | junteng_dev | `4ab0c541` | customer-portal, help-desk | implementing |
| Junxian | junxian_dev | `96bbc11b` | currency-converter, social-connector | implementing |
| Lueyang | lueyang-dev | `bf8b9a1d` | territory-manager, client-portal | implementing |
| Lv | lv | `0be3288e` | survey-builder, analytics-dashboard, insights-engine | implementing |
| Ruige | ruige | `a6f3fba3` | web-crawler, log-analyzer | implementing |
| Wenshuo | wenshuo-dev | `d11080ed` | cache-optimizer, scheduler | implementing |
| Xiaochen | xiaochen_dev | `20bfc734` | status-checker, health-monitor | implementing |
| Yuxuan | yuxuan-dev | `e82c8295` | sync-service | implementing |
| Yuzhen | yuzhen-dev | `c68265da` | audit-logger, resource-monitor | implementing |
| Zhaochen | zhaochen | `b5f9cc4f` | certificate-manager, storage-manager | implementing |

Notes:
- `main` carries no new task commits (only the example template).
- Yuxuan's latest commit also places a stray set of files directly under
  `tasks/yuxuan/` (not inside a task directory); those are not valid tasks
  (`task_dir` must be `split/task_name`).
- Lueyang's latest commit only touches `territory-manager`
  (`client-portal` files were added in an earlier commit); both are still
  missing `task_config.json`.

## Promotion checklist

Add a `task_config.json` next to the task's `docs/` folder, e.g.:

```json
{
  "needed_mcp_servers": ["filesystem", "github"],
  "needed_local_tools": ["claim_done"]
}
```

Then verify:
1. `task_config.json` parses, `needed_mcp_servers` is non-empty, and
   `needed_local_tools` contains `claim_done` (the framework exposes it as
   `local-claim_done`).
2. `docs/task.md` and `docs/agent_system_prompt.md` are non-empty and English only.
3. `docs/user_system_prompt.md`, if present, is English only.
4. The task runs end-to-end (`uv run python main.py --task_dir tasks/<split>/<task>`)
   and passes its `evaluation/main.py` check.

Once a task passes, copy it into `tasks/finalpool/<task_name>/` on this branch.
