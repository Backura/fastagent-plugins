---
description: Analyze error and trace execution path to identify root cause
argument-hint: [error-message-or-file]
allowed-tools: Read, Grep, Glob, Bash(git log:*), Bash(git diff:*)
---

# Debug Trace Analysis

Investigate the issue: $ARGUMENTS

## Analysis Steps

1. **Understand the Error**:
   - Parse error message and stack trace
   - Identify failure point
   - Note error type and context

2. **Trace Execution Path**:
   - Follow code flow leading to error
   - Identify relevant functions/methods
   - Check variable states and conditions

3. **Form Hypotheses**:
   - List possible root causes
   - Prioritize by likelihood
   - Consider recent changes: !`git log --oneline -10`

4. **Identify Root Cause**:
   - Explain what went wrong
   - Explain why it happened
   - Show evidence supporting diagnosis

5. **Suggest Fix**:
   - Provide specific code changes
   - Explain why fix works
   - Suggest tests to prevent recurrence

Present findings clearly with:
- Root cause explanation
- Evidence (stack trace, code snippets)
- Recommended fix
- Prevention strategy
