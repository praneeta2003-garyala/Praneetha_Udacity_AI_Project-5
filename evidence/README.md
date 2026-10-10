# Evidence Index

## System 1

- `system1/task1_pytest_output.txt` — captured test run, 29 passed.
- `system1/runs/20261007_184934/summary.md` — eight fixture outcomes and estimated cost.
- `system1/runs/20261007_184934/traces/` — per-claim traces.
- `system1/runs/20261007_184934/queues/` and `escalations.jsonl` — routed/escalated outcomes.

Verified summary: 7 routed, 1 escalated, 0 incomplete.

## System 2

- `system2/task1_pytest_output.txt` — 30 passed.
- `system2/runs/20261008-162025/budget.json` — baseline and assembled token budget.
- `context.md`, `eval.jsonl`, `eval_control.jsonl`, and `case_facts_call.json` — assembled prompt and evaluations.

Verified summary: 38,708 baseline tokens, 16,759 assembled tokens, 56.7% reduction, full evaluation 6/6 passed, control regression observed.

## System 3

- `system3/task1_pytest_output.txt` — 35 passed.
- `system3/validator_output.txt` — `OK`.
- `system3/validator_exit_code.txt` — the captured validator process exit status, `0`.
- `system3/CLAUDE.md`, `system3/.claude/`, and `system3/docs/plan-mode-vs-direct-execution.md` — configuration evidence.

The `.claude/` files are intentionally included here, not only in the source folder, so the evidence path referenced by the README is real.

## System 4

- `system4/task1_pytest_output.txt` — 33 passed.
- `system4/task2_shift_output.txt` — recorded-response Shift C output; it reports `new=0` for the captured `since` time.
- `system4/task3_sql_slice_output.txt` — separate query of 13 historical Shift C records from the warm database.
- `system4/hot_state.json` — 643 bytes, valid JSON.
- `system4/shift_scratchpad.jsonl` — generated scratchpad record.
- `system4/warm.sqlite` — warm tier database used for the SQL evidence.

## Program budget screenshot

- `program_budget.png` — dashboard showed $19.30 remaining out of $20.00 (approximately $0.70 consumed at capture time). This is program-level context; System 1's $0.2089 is a separate run estimate included within overall spend, not an additional amount.
