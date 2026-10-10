# Reflection Brief — Harness Engineering Capstone

# 

# Name:Praneeta Garyala

# Date:09-Oct-2026

# 

# 

# Environment

# 

# \- Model(s):System 1 full run evidence is associated with `claude-haiku-4-5-20251001`; System 2's `case\\\_facts\\\_call.json` records its model. System 4 used a recorded offline response for Shift C.

# 

# \- OS / Python: Linux / Python 3.13.0,

# \- Approx. API spend: $0.70 consumed across the program. $19.30 remaining from a $20.00 budget.

# 

# \---

# 

# \## Part 1 — Per-system

# 

# \### System 1 — Agentic loop

# 

# 1\. Loop control

# &#x20;  → The trace `evidence/system1/runs/20261007\\\_184934/traces/claim\\\_02\\\_stolen\\\_bike.jsonl` records the stop-reason progression `tool\\\_use → tool\\\_use → tool\\\_use → tool\\\_use → end\\\_turn`. In `solutions/system1/claims\\\_intake/loop.py`, `run()` continues the loop when the response stop reason is `tool\\\_use`, executes the requested tools, and returns when it sees `end\\\_turn`. It raises `UnexpectedStopReason` for an unrecognized stop reason instead of guessing what the model intended. This makes the API's control signal—not the wording of an assistant message—the basis for continuing or ending the claim workflow.

# 

# 2\. Anti-pattern

# &#x20;  → `solutions/system1/tests/test\\\_antipatterns.py` checks against brittle patterns such as searching assistant text for a word to decide whether to stop, or using a fixed integer iteration cap as the primary loop-control mechanism. The full run `evidence/system1/runs/20261007\\\_184934/summary.md` shows that claims needed different turn counts, including eight turns for `claim\\\_06\\\_low\\\_confidence\\\_escalation`. A fixed cap could stop the escalation flow before its terminal action, while text matching could mistake a normal explanation for a control instruction. The tests help keep the loop contract explicit and resilient to variation in model wording.

# 

# 

# 3\. Tool design

# &#x20;  → In `solutions/system1/claims\\\_intake/tools.py`, `classify\\\_claim` determines the type of claim, while `assess\\\_severity` assesses how serious it is; the descriptions and input schemas define different outputs even though both operate on claim facts. The terminal tools `route\\\_to\\\_adjuster` and `escalate\\\_to\\\_human` also represent different end states. The tool error structure carries fields such as `is\\\_error`, `error\\\_category`, `is\\\_retryable`, and `message`, rather than leaving an error as an untyped sentence. This allows the loop/model to distinguish a recoverable issue from a permanent validation problem and decide whether a retry or a different action is appropriate.

# 

# 4\. Your numbers

# &#x20;  →

# 

# \### System 2 — Context strategy

# 

# 5\. The reduction

# &#x20;  → `evidence/system2/runs/20261008-162025/budget.json` records a baseline of 38,708 tokens and an assembled context of 16,759 tokens, a 56.7% reduction. The `active` section dominates at 15,789 tokens, compared with `case\\\_facts` at 204, `resolved\\\_refund` at 347, and `resolved\\\_subscription` at 437. Keeping the active issue verbatim preserves the latest details and wording most likely to determine the answer. The smaller resolved sections can be compacted because their purpose is to preserve settled facts rather than the exact flow of the active exchange.

# 

# 6\. Summarize vs preserve.

# &#x20;  → The section counts in `evidence/system2/runs/20261008-162025/budget.json` show that the case-facts block is 204 tokens and the resolved-refund and resolved-subscription summaries are 347 and 437 tokens. The active issue is 15,789 tokens and is retained verbatim in the assembled context. The strategy is to summarize settled history and durable facts, but preserve the active conversation exactly when specific phrasing or current details might matter. A compact facts block also provides stable identifiers and values after verbose conversation history has been summarized.

# 

# 7\. Facts block.

# &#x20;  → In `evidence/system2/runs/20261008-162025/eval.jsonl`, the full assembled context passes 6/6 questions, including the exact Q6 status value `in\\\_progress`. In `eval\\\_control.jsonl`, Q6 fails when that exact structured status is not preserved; Q1 also unexpectedly passes in the control run. The Q6 difference supports the idea that compact structured facts can carry a critical value more reliably than a summary of narrative alone. Because Q1 unexpectedly passes in the control, I would describe the control as showing a regression but not as a perfect isolated experiment.

# 

# \### System 3 — Claude Code config

# 

# 8\. Path-scoped rules.

# &#x20;  → The file `evidence/system3/.claude/rules/api.md` has YAML frontmatter with `paths: - "src/api/\\\*\\\*/\\\*"`. That applies API-handler guidance to matching API files rather than imposing it on unrelated frontend or test work. The other rules scope React guidance to component/page paths and test conventions to test-file globs. This complements project-level `CLAUDE.md`: cross-cutting standards can be imported globally, while specialized conventions appear only when the current file path calls for them.

# 

# 

# 9\. Forked skill

# &#x20;  → In `evidence/system3/.claude/skills/deploy-check/SKILL.md`, the frontmatter sets `context: fork` and an `allowed-tools` list limited to read-oriented tools such as `Read`, `Grep`, `Glob`, and read-only Git/GitHub commands. The fork gives the review or deployment-check workflow an isolated context so verbose inspection doesn't crowd the main conversation. The read-only allowlist limits accidental side effects: the skill can inspect status and checks, but it isn't granted permission to edit files, push commits, or deploy. Without this boundary, an inspection workflow could mutate the very repository state it is supposed to assess.

# 

# 

# 10\. Scope

# &#x20;   → `evidence/system3/CLAUDE.md` describes the repository's `./CLAUDE.md`, `.claude/standards/`, and `.claude/rules/` as project-level configuration that can be shared with the team. It contrasts this with personal locations such as `\\\~/.claude/CLAUDE.md` and `\\\~/.claude/commands/`, which belong to the user's own setup rather than the repository. The validator output in `evidence/system3/validator\\\_output.txt` is `OK` with exit code 0, while `evidence/system3/task1\\\_pytest\\\_output.txt` shows 35 passing tests. The scope distinction keeps team standards reviewable and version-controlled without forcing every developer to use the same personal preferences.

# 

# 

# \### System 4 — Orchestration

# 

# 11\. Push work down.

# &#x20;   → The SQL slice `evidence/system4/task3\\\_sql\\\_slice\\\_output.txt` returns 13 historical Shift C records, filtered in SQLite with `WHERE shift = 'C'` and ordered by `ts`. The fixture contains 40 records across shifts, while `solutions/system4/shift\\\_monitor/warm.py` defines the indexed warm-store query `SELECT \\\* FROM defects WHERE ts > ? ORDER BY ts DESC LIMIT ?` in `defects\\\_since()`, supported by timestamp and shift/timestamp indexes. The point is to let SQL select the relevant bounded slice before records are passed into the model-facing prompt, rather than loading the complete historical table into every request. The separate recorded run in `evidence/system4/task2\\\_shift\\\_output.txt` says `new=0` because its `since` timestamp is in October 2026; that value must not be confused with the separate historical 13-row query.

# 

# 

# 12\. Crash recovery

# &#x20;   → `solutions/system4/shift\\\_monitor/recovery.py` defines `STALE\\\_RESUME\\\_THRESHOLD\\\_MINUTES = 30` and uses `decide()` to select whether an incomplete run should resume or start fresh. The policy resumes recent incomplete state, but chooses a fresh run when there is no usable step, the prior run is complete, or the partial state is older than the 30-minute threshold. Tests in `solutions/system4/tests/test\\\_us03\\\_crash\\\_recovery.py` cover values at 30 and 31 minutes, and the captured suite in `evidence/system4/task1\\\_pytest\\\_output.txt` has 33 passing tests. The reason for not blindly resuming stale context is that a partial transcript may no longer represent current conditions; a fresh prompt can use the durable manifest and summary instead.

# 

# 

# 13\. Small state

# &#x20;   → The produced `evidence/system4/hot\\\_state.json` is 643 bytes, below the 5,120-byte budget tested in `solutions/system4/tests/test\\\_us01\\\_tiered\\\_state.py`. That test file also limits the number of recent hashes, preventing hot state from growing without bound. The warm SQLite tier and cold summaries provide ways to retain and query history without putting all history into the hot state sent through each shift. For a process that runs every shift indefinitely, a fixed state budget helps keep prompts bounded and predictable over time.

# 

# \---

# 

# \## Part 2 — Synthesis

# 

# \*Graded on connecting two or more systems. Cite a named file/artifact from each.\*

# 

# 14\. Three layers.

# \- Model: evidence/system1/runs/20261007\_184934/traces/claim\_02\_stolen\_bike.jsonl demonstrates the model layer because the trace records the model requesting tool calls to process the claim and eventually returning end\_turn. It shows the model making decisions based on the claim information and available tools.

# \- Harness: solutions/system1/claims\_intake/loop.py, particularly the ru	n() function, represents the harness layer. It enforces the control-flow contract: continue on tool\_use, stop on end\_turn, and raise an error for unexpected stop reasons. This makes loop termination deterministic instead of relying on interpreting the model's natural-language response.

# \- Orchestration: solutions/system4/shift\_monitor/pipeline.py, together with solutions/system4/shift\_monitor/warm.py and solutions/system4/shift\_monitor/recovery.py, represents the orchestration layer. These components coordinate the shift workflow, retrieve relevant defect records from SQLite, and manage recovery decisions for interrupted runs. The execution and supporting state are evidenced by evidence/system4/task2\_shift\_output.txt and evidence/system4/task3\_sql\_slice\_output.txt.

# 

# 15\. Deterministic vs prompt

# &#x20;   → One deterministic guarantee is in `solutions/system1/claims\\\_intake/loop.py`: `end\\\_turn` ends the loop, `tool\\\_use` continues it, and an unexpected reason raises an error. Another is the hot-state byte limit tested in `solutions/system4/tests/test\\\_us01\\\_tiered\\\_state.py`. By contrast, `solutions/system1/claims\\\_intake/system\\\_prompt.py` and the tool descriptions guide the model's approach to gathering facts, classifying, assessing severity, and routing claims.

# 

# 16\. Context, two faces

# &#x20;   → System 2 manages context within a support conversation: `evidence/system2/runs/20261008-162025/budget.json` shows a reduction from 38,708 to 16,759 tokens, or 56.7%, while retaining the 15,789-token active section verbatim. System 4 manages operational state across shifts: `evidence/system4/hot\\\_state.json` is 643 bytes, and `task3\\\_sql\\\_slice\\\_output.txt` demonstrates querying 13 historical Shift C records from warm storage. The shared principle is to keep the immediately useful state compact while preserving access to authoritative details. The difference is that System 2 assembles a session context, while System 4 uses tiered persistence, SQL retrieval, and recovery logic.

# 

# 

# 17\. Reliability you can't see in one run

# &#x20;   → The tests in `solutions/system4/tests/test\\\_us04\\\_fork\\\_scratchpad.py` verify that forking copies state without mutating the base and that two forks retain independent scratchpads. A single successful run would not prove this isolation property, because it might never exercise two hypotheses concurrently or check the original state afterward. The suite evidence `evidence/system4/task1\\\_pytest\\\_output.txt` records 33 passing tests, including those fork tests. This matters before shipping because a shared mutable scratchpad could let one hypothesis contaminate another and make later decisions depend on unrelated exploratory work.

# 

# 18\. Blast radius

# &#x20;   → For System 1, the main risk is an incorrect claim being routed to an adjuster or incorrectly escalated, affecting downstream claim handling. The action boundary is visible in `solutions/system1/claims\\\_intake/tools.py`, where `route\\\_to\\\_adjuster` and `escalate\\\_to\\\_human` are the terminal actions; `solutions/system1/claims\\\_intake/loop.py` also enforces budget and stop-reason checks. The verified summary `evidence/system1/runs/20261007\\\_184934/summary.md` reports seven routed claims, one escalation, and no incomplete claims. I would not claim a separate emergency kill switch exists unless one is present in the implementation; the verified controls limit the loop and define explicit terminal actions.

# 

# \---

# 

# \## Part 3 — Honest assessment

# 

# 19\. What broke

# &#x20;   → During verification, my first SQL check failed because I used `detected\\\_at`, a column that does not exist in the `defects` table. Inspecting the schema showed the actual timestamp field is `ts`, so I corrected the query to use that field. The corrected query is captured in `evidence/system4/task3\\\_sql\\\_slice\\\_output.txt` and returned 13 historical Shift C rows. The experience reinforces that queries should be checked against the real schema rather than relying on an assumed field name.

# 

# 20\. What you'd change

# &#x20;   → The recorded run output in `evidence/system4/task2\\\_shift\\\_output.txt` shows a `since` timestamp of `2026-10-08T09:41:15Z` and `new=0`, while the recorded response describes Shift C on `2026-04-30`. A design improvement would be to let offline replay specify an explicit `as-of` timestamp aligned with the fixture and include that timestamp in the output. That would make the intended time window reproducible and avoid readers confusing a historical recorded response with freshly detected defects. I would add a test proving that the supplied replay timestamp is used consistently by SQL filtering and the generated state.



