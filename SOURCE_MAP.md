# Source and Evidence Map

| System | Source and tests | Runtime / verification evidence |
|---|---|---|
| 1 | `solutions/system1/claims_intake/`, `solutions/system1/tests/` | `evidence/system1/task1_pytest_output.txt`; `evidence/system1/runs/20261007_184934/` |
| 2 | `solutions/system2/retail_context/`, `solutions/system2/tests/` | `evidence/system2/task1_pytest_output.txt`; `evidence/system2/runs/20261008-162025/` |
| 3 | `solutions/system3/CLAUDE.md`, `solutions/system3/.claude/`, tests | `evidence/system3/task1_pytest_output.txt`; validator outputs; `evidence/system3/.claude/` |
| 4 | `solutions/system4/shift_monitor/`, `solutions/system4/tests/` | `evidence/system4/task1_pytest_output.txt`; Shift C output; SQL slice; hot state; scratchpad |

Program budget context: `evidence/program_budget.png`.

Canonical runtime evidence lives under `evidence/`. The source copy retains only the successful System 1 run and the canonical System 2 run, avoiding older incomplete attempts.
