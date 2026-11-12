# Functional Clusters and Workflow Patterns

## Ten clusters
1. Research & Strategy
2. Requirements & Architecture
3. Development & Debugging
4. Testing & QA
5. DevOps & Release
6. Operations
7. Data & Traditional ML
8. GenAI Engineering
9. GenAI Evaluation & Safety
10. AI Production & Lifecycle

## Execution patterns
- Sequential: use when a task depends on an earlier artifact (requirements → design → implementation).
- Parallel: use only when tasks have independent inputs/outputs (research sources, independent test suites, backend/frontend after contracts are stable).
- Fan-out/fan-in: delegate independent work, then merge and validate results.
- Feedback loop: failed quality gate routes work to the responsible upstream agent with evidence.
- Human checkpoint: required for scope approval, stack approval, production release, destructive migration, security-sensitive action and model promotion.

## Example RAG DAG
1. Discovery and requirements.
2. In parallel: corpus/data assessment and model/provider research.
3. RAG architecture and data contracts.
4. In parallel: document ingestion/indexing and LLM client implementation where interfaces are fixed.
5. Build retrieval + generation integration.
6. In parallel: retrieval evaluation, generation evaluation, security tests and performance tests.
7. Aggregate results; fix failures; rerun affected tests.
8. Human approval; deploy; monitor.

## DAG node contract
Each node should define: task_id, agent_id, tier, input_refs, dependencies, acceptance_criteria, timeout, retry_policy, permissions, output_schema and status.

## Conflict handling
Do not allow concurrent agents to edit the same files unless work is partitioned or isolated in separate branches/worktrees. Prefer parallel research and test execution; serialize shared-file edits and merges.
