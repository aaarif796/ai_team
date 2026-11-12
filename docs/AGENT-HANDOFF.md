# Agent Handoff Contract

Agents should exchange concise structured task packets, not entire chat histories.

## Required fields
```json
{
  "task_id": "TASK-0001",
  "source_agent": "requirements-analyst",
  "target_agent": "system-architect",
  "status": "completed",
  "tier_used": "mid",
  "summary": "Validated requirements are ready for design.",
  "input_refs": ["docs/requirements.md"],
  "output_refs": ["docs/requirements.md"],
  "acceptance_criteria": ["All requirements have IDs", "NFRs are measurable"],
  "evidence": ["validation-report.md"],
  "assumptions": [],
  "risks": [],
  "attempt": 1,
  "requires_human_approval": false,
  "next_agent": "system-architect"
}
```

## Status values
`pending`, `running`, `completed`, `partial`, `blocked`, `failed`, `cancelled`.

## Rules
- Use repository paths or stable artifact identifiers for references.
- Include exact commands and exit codes when tests/tools run.
- Never report a tool as executed if it was only recommended.
- Include unresolved issues and required human decisions.
- Avoid passing secrets or unnecessary personal data between agents.
