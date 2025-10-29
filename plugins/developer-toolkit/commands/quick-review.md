---
description: Quick code review focusing on critical issues (security, bugs, performance)
argument-hint: [file-or-directory]
allowed-tools: Read, Grep, Glob
---

# Quick Code Review

Perform a focused code review on $ARGUMENTS, prioritizing:

1. **Critical Issues** (must fix):
   - Security vulnerabilities
   - Logic errors and bugs
   - Performance problems
   - Breaking changes

2. **Important Issues** (should fix):
   - Missing error handling
   - Inadequate test coverage
   - Poor naming or unclear code

Skip style preferences and minor optimizations. Provide:
- Clear issue descriptions
- Severity level (critical/important)
- Specific fix suggestions
- Line numbers where applicable

Keep feedback concise and actionable.
