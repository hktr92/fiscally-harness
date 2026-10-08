---
name: issue-execution
description: Execute a ready file-based issue through investigation, implementation, validation and commit.
---

# issue-execution

1. Read root and target AGENTS files; read the complete issue in `docs/issues/1-new/`.
2. Confirm acceptance criteria, inspect Git status, and discover the touched stack.
3. Move the issue into `2-in-progress/` before coding, preserving filename.
4. Make minimal scoped changes, adding regression tests where meaningful.
5. Run configured relevant gates. Record exact results; distinguish inherited failures.
6. If acceptance criteria are met, complete the handoff section and move to `3-done/`.
7. Commit only the issue and task-owned files; report SHA and outstanding risks. If blocked, keep `2-in-progress/` and explain. No forced subagents.
