# Harness Engineering Capstone — Submission Package

This package contains the four solution workspaces, captured test/runtime evidence, the official-format reflection brief, and a file manifest. The root `reflection-brief.md` follows the official 20-question structure and contains an evidence-grounded working draft of the answers. Before uploading, review the language, confirm it expresses your understanding, complete the explicitly marked System 1 README-sample comparison in Question 4, and enter your course-registered name.

## System 1 — Claims Intake Agent

- Source and tests: `solutions/system1/`
- Test evidence: `evidence/system1/task1_pytest_output.txt` (**29 passed**)
- Successful full run: `evidence/system1/runs/20261007_184934/`
- Result: 8 fixtures, 7 routed, 1 escalated, 0 incomplete.
- Estimated full-run cost: `$0.2089` in the run summary.

## System 2 — Retail Support Copilot Context Strategy

- Source and tests: `solutions/system2/`
- Test evidence: `evidence/system2/task1_pytest_output.txt` (**30 passed**)
- Run: `evidence/system2/runs/20261008-162025/`
- Budget evidence: 38,708 baseline tokens; 16,759 assembled tokens; 56.7% reduction.
- Full evaluation: 6/6 passed. The control evaluation includes a regression (Q6).

## System 3 — Claude Code Multi-Surface Monorepo Team

- Source and tests: `solutions/system3/`
- Configuration appears in both `solutions/system3/.claude/` and `evidence/system3/.claude/`.
- Test evidence: `evidence/system3/task1_pytest_output.txt` (**35 passed**)
- Validator: `evidence/system3/validator_output.txt` (`OK`); exit status in `evidence/system3/validator_exit_code.txt` (`0`).
- Plan-mode document: `evidence/system3/docs/plan-mode-vs-direct-execution.md`.

## System 4 — Multi-Shift Quality Monitoring

- Source and tests: `solutions/system4/`
- Test evidence: `evidence/system4/task1_pytest_output.txt` (**33 passed**)
- Recorded run: `evidence/system4/task2_shift_output.txt`
- SQL slice: `evidence/system4/task3_sql_slice_output.txt` (13 historical Shift C records)
- Hot state: `evidence/system4/hot_state.json` (**643 bytes**, valid JSON)
- Scratchpad: `evidence/system4/shift_scratchpad.jsonl`
- Important distinction: the recorded shift run reports `new=0` for its October `since` timestamp. The 13-row SQL output is a separate historical Shift C query, not a claim that those rows were new in the recorded run.

## API budget evidence

`evidence/program_budget.png` shows $19.30 remaining from a $20.00 program budget (approximately $0.70 consumed at the screenshot time). This is a program-level approximate balance and should not be added to System 1's `$0.2089` run estimate.

## Reflection

- `reflection-brief.md` follows the 20-question structure from the official template and includes evidence-grounded draft answers.
- Review every response to make sure it matches your understanding. Question 4 is deliberately flagged because the parent exercise README sample values were not provided in the uploaded workspace bundle.

## Package hygiene

Virtual environments, Python bytecode caches, pytest caches, and package-install metadata directories are excluded. Older incomplete System 1 run folders were removed from the source copy; the canonical successful run is retained in the evidence folder and matching selected source run.
