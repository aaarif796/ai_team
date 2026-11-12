# OpenCode Multi-Agent Engineering Framework — Complete User Manual

> This manual explains the agents, skills, commands, tools, model tiers, workflows, permissions,
> practical usage patterns, troubleshooting, and the limits of this package.
>
> **Package version:** 1.0  
> **Target:** OpenCode project-local setup  
> **Contents:** 44 agents, 119 skills, 4 slash commands

---

## 1. What this framework is

This framework is a collection of specialist agent definitions, reusable skill instructions,
and slash-command workflow templates for OpenCode. It is intended to support the full
engineering lifecycle:

- Requirements and planning
- Architecture and technology selection
- Implementation and refactoring
- Test-driven development and all major testing levels
- Debugging, root-cause analysis, and code review
- Security, performance, accessibility, and dependency review
- CI/CD, release, operations, and incident response
- Data engineering and machine learning
- Model training, evaluation, monitoring, drift, and retraining
- GenAI, prompt engineering, agents, and RAG engineering/evaluation

### Important: what it does not do by itself

This package is **not** a separate autonomous orchestration server. It does not include a
custom Python scheduler, durable DAG engine, database-backed shared state, automatic
provider failover, cost accounting service, or guaranteed retry worker.

OpenCode supplies the agent runtime and tool execution. The Markdown files provide roles,
instructions, skills, and workflows. Retry/escalation policies in the prompts are instructions
to the model, not hard runtime guarantees.

The current agent and skill files are starter definitions. Agent roles have specific missions,
but share common operating rules. The 119 skills are reusable starter playbooks with a common
procedure and quality checklist; they are not 119 fully researched, domain-specific textbooks.
Treat them as a structured foundation that you can deepen as your team develops.

---

## 2. Folder structure

```text
project-root/
├── opencode.json
├── .env.example
├── .opencode/
│   ├── agents/                 # 44 specialist agent definitions
│   ├── skills/                 # 119 reusable SKILL.md playbooks
│   └── commands/               # 4 slash-command workflows
└── docs/
    ├── README_ALL_AGENT_SKILL_AND_TOOL.md
    ├── AGENT_INVENTORY.md
    ├── SKILL_INVENTORY.md
    ├── MODEL_SETUP.md
    └── WORKFLOW_AND_LIMITS.md
```

### What each directory does

| Path | Purpose | When you work here |
|---|---|---|
| `opencode.json` | OpenCode project configuration and default orchestrator selection | Provider setup, permissions, default agent |
| `.opencode/agents/` | Agent identity, model, mode, and role instructions | Add or tune a specialist |
| `.opencode/skills/` | Reusable task procedures | Add domain-specific working knowledge |
| `.opencode/commands/` | Reusable slash-command prompts | Create repeatable workflows |
| `docs/` | Human documentation and inventories | Learn, audit, and maintain the framework |
| `.env.example` | Names of environment variables to configure locally | Set up provider credentials safely |

Do not put real API keys in Git. Copy the variable names into your local environment or
secret manager and provide the actual values there.

---

## 3. How to install and start

1. Extract the ZIP into the root of the repository where you want OpenCode to work.
2. Confirm that `opencode.json` and `.opencode/` are directly inside that project root.
3. Configure your provider and model IDs. Read `docs/MODEL_SETUP.md`.
4. Set provider API keys in your local environment.
5. Open a terminal in the project root and start OpenCode:

```bash
opencode
```

6. Check the installed OpenCode version and its current configuration behavior. OpenCode
   configuration evolves; if your installed version reports an unknown field or ignores an
   agent, use the documentation for that exact version and update the config shape.
7. Try a read-only exploration prompt first, then run a workflow command.

### First safe test

```text
Explore this repository. Do not edit files.
Explain the application architecture, entry points, test commands, and important risks.
Tell me which specialist agents and skills would be useful for the next task.
```

This validates repository access and agent discovery without asking for code changes.

---

## 4. Agents: what they are and how to use them

An **agent** is a specialist AI role. Its Markdown file defines its name/ID, description,
mode, model, and instructions.

### Primary agent vs subagent

- **Primary agent:** the main assistant for the current session. It receives your request,
  coordinates work, and returns the final result.
- **Subagent:** a specialist child session delegated a bounded task by the primary agent.
  It returns findings or completed work to the parent.
- **This package:** `sdlc-orchestrator` is configured as the primary agent. The other
  43 agents are specialist subagents.

In OpenCode, use the primary-agent switcher (commonly Tab in the TUI) to change primary
agents. Subagents can generally be invoked by mentioning their ID with `@`, for example
`@debugging-agent`, or the orchestrator can delegate to them through OpenCode's subagent
mechanism when permitted. Exact UI/key behavior can vary by OpenCode version and client.

### How to delegate effectively

A weak request is:

```text
@debugging-agent fix my bug
```

A stronger request is:

```text
@debugging-agent
Investigate the login failure. Do not modify files yet.
Start from POST /api/login and trace the request through validation,
authentication, database access, and response serialization.
Return:
1. Reproduction steps
2. Exact files/functions involved
3. Root-cause hypothesis with evidence
4. A regression test proposal
5. The smallest safe fix
Do not guess line numbers or claim tests were run.
```

A good delegation always contains:

1. **Objective:** the exact question or deliverable.
2. **Context:** relevant files, logs, error messages, architecture, or constraints.
3. **Scope:** what the agent may and may not change.
4. **Output format:** findings, patch, test plan, table, or decision record.
5. **Acceptance criteria:** what makes the work complete.
6. **Validation:** commands or evidence expected.

### Full agent directory

| Agent ID | Default tier | Use it when… | Expected output |
|---|---:|---|---|
| `sdlc-orchestrator` | High | A task crosses multiple engineering stages or needs coordination | Plan, delegation, integrated result, risks |
| `requirements-validator` | Small | A request is ambiguous or acceptance criteria are missing | Clarified requirements, edge cases, acceptance criteria |
| `project-planner` | Small | Work needs milestones, sequencing, or dependencies | Task breakdown, milestones, dependencies |
| `solution-architect` | High | You need system design, component boundaries, or trade-off analysis | Architecture proposal, data flows, decisions |
| `technology-selector` | High | Choosing a framework, database, model, library, or infrastructure | Evidence-based comparison and recommendation |
| `developer` | Mid | Implementing a bounded feature or change | Code changes and implementation notes |
| `tdd-agent` | Mid | Tests should be designed before implementation | Failing test proposal, red-green-refactor plan |
| `test-engineer` | Mid | Planning or running a broad test strategy | Test matrix, test results, coverage gaps |
| `qa-automation` | Mid | Automating repeatable tests or CI checks | Automation code and execution strategy |
| `debugging-agent` | High | An error must be reproduced and traced to root cause | Reproduction, evidence, root cause, fix options |
| `code-reviewer` | High | Reviewing a diff before merge or release | Findings ordered by severity, file references, risks |
| `security-reviewer` | High | Reviewing auth, secrets, data exposure, inputs, or attack surface | Threats, severity, remediation |
| `performance-engineer` | High | Latency, throughput, memory, CPU, or database performance is poor | Profiling plan, bottlenecks, measurable improvements |
| `database-engineer` | Mid | Schema, indexes, queries, transactions, or migrations need work | Schema/query/migration proposal |
| `api-engineer` | Mid | Designing or implementing API endpoints and contracts | Endpoint contract, validation, error behavior |
| `devops-engineer` | Mid | CI/CD, containers, deployment, or infrastructure needs work | Pipeline/configuration and rollout notes |
| `documentation-agent` | Small | Creating or updating technical docs and runbooks | Structured documentation |
| `release-manager` | Mid | Preparing a release, version bump, rollout, or rollback | Release checklist, notes, rollback plan |
| `python-engineer` | Mid | Python-specific implementation, packaging, typing, or debugging | Python code and quality checks |
| `backend-engineer` | Mid | Server-side business logic or service integration | Backend implementation and tests |
| `frontend-engineer` | Mid | UI, client state, browser behavior, or frontend integration | UI implementation and validation |
| `data-engineer` | Mid | Data ingestion, transformation, pipelines, or storage | Pipeline design and data checks |
| `ml-architect` | High | Designing an end-to-end ML system | ML architecture, data/model/deployment plan |
| `ml-engineer` | Mid | Implementing preprocessing, training pipeline, or inference | Reproducible ML pipeline/code |
| `data-scientist` | High | Exploratory analysis, statistics, feature analysis, or experiments | Analysis, hypotheses, statistical findings |
| `model-training-agent` | High | Training or fine-tuning a model | Training plan/configuration and reproducibility notes |
| `model-evaluation-agent` | High | Defining model metrics, baselines, or acceptance gates | Evaluation protocol, metrics, error analysis |
| `model-monitoring-agent` | Mid | Monitoring production model quality, latency, cost, or drift | Monitoring plan, metrics, alerts |
| `model-drift-agent` | High | Investigating data drift or concept drift | Drift diagnosis and validated response options |
| `rag-architect` | High | Designing a RAG system or retrieval strategy | RAG architecture and design decisions |
| `rag-engineer` | Mid | Implementing parsing, chunking, embeddings, retrieval, reranking, generation | Working RAG pipeline |
| `rag-evaluation-agent` | High | Measuring retrieval and answer quality | Dataset, metrics, failure analysis, comparison |
| `prompt-engineer` | Mid | Improving prompts, output schemas, or prompt tests | Versioned prompt and test cases |
| `llm-evaluation-agent` | High | Comparing LLMs for quality, robustness, safety, latency, or cost | Evaluation design and model comparison |
| `agent-engineer` | High | Designing tools, agent handoffs, permissions, and agent workflows | Agent architecture and tool contracts |
| `data-quality-agent` | Mid | Checking data correctness, completeness, leakage, or lineage | Data quality report and remediation |
| `experiment-tracker` | Small | Recording experiment settings and results | Reproducible experiment summary |
| `observability-agent` | Mid | Adding logs, metrics, traces, and alerts | Instrumentation and dashboard/alert plan |
| `migration-agent` | High | Planning risky database, API, or application migrations | Migration sequence, compatibility, rollback |
| `incident-responder` | High | Production outage or operational incident | Triage, impact, mitigation, post-incident actions |
| `accessibility-reviewer` | Mid | Checking UI accessibility and inclusive interaction | Accessibility findings and fixes |
| `dependency-auditor` | Mid | Auditing package versions, licenses, vulnerabilities, upgrades | Dependency risk report |
| `technical-researcher` | High | Investigating external technical options or documentation | Source-backed research summary |
| `acceptance-reviewer` | High | Verifying completed work against original requirements | Pass/fail acceptance report and gaps |

### Which agents should work together?

| Task | Recommended sequence |
|---|---|
| New feature | Requirements Validator → Architect (if needed) → TDD Agent → Developer → Test Engineer → Code Reviewer → Acceptance Reviewer |
| Bug fix | Debugging Agent → TDD Agent → Developer → Test Engineer → Code Reviewer |
| Security issue | Security Reviewer → Debugging Agent/Developer → Test Engineer → Security Reviewer |
| Slow API | Performance Engineer → Backend/API Engineer → Test Engineer → Code Reviewer |
| Database migration | Database Engineer → Migration Agent → Test Engineer → Security/Code Reviewer |
| New ML model | ML Architect → Data Scientist/Data Quality → Model Training → Model Evaluation → Model Monitoring |
| RAG system | RAG Architect → RAG Engineer → RAG Evaluation → LLM Evaluation → Observability |
| Agent system | Agent Engineer → Prompt Engineer → LLM Evaluation → Security Reviewer |
| Production incident | Incident Responder → Relevant domain engineer → Test Engineer → Observability Agent → Documentation Agent |

Do not invoke every agent for every task. Choose the smallest team that covers the risk.
For a small typo, a developer plus a quick check is enough. For a database migration or
security-sensitive change, use more independent review.

---

## 5. Skills: what they are and how to use them

A **skill** is reusable procedural knowledge. It is not an independent agent and does not
run commands by itself. A skill tells an agent how to approach a particular kind of work.

OpenCode discovers skills from `.opencode/skills/<skill-id>/SKILL.md`. The directory name
is the skill ID. The agent can load a skill when it is relevant, or you can explicitly ask
for it using `@skill-id`. The exact discovery and invocation behavior depends on the
OpenCode version.

### How to use a skill

Example:

```text
Use @test-driven-development and @pytest.
Inspect the existing tests, write a failing regression test for this bug first,
then implement the smallest fix and run the relevant tests.
```

Another example:

```text
Use @rag-architecture, @hybrid-search, and @ragas-evaluation.
Review my current RAG pipeline and propose an evaluation plan before changing code.
```

Best practice:

1. Ask the agent to load the relevant skill(s).
2. Give it repository-specific context.
3. Tell it whether to investigate, plan, or modify files.
4. Ask it to follow the skill's procedure.
5. Require evidence: tests, metrics, source references, or file changes.
6. Review the output. A skill is guidance, not proof that the work is correct.

### All 119 skills, grouped by purpose

#### Requirements, planning, architecture
| Skill ID | Use it for |
|---|---|
| `requirements-discovery` | Eliciting missing functional and non-functional requirements |
| `acceptance-criteria` | Turning requirements into testable completion conditions |
| `user-story-mapping` | Mapping users, journeys, and feature slices |
| `technical-planning` | Breaking technical work into ordered tasks |
| `architecture-decision-records` | Recording architecture decisions and trade-offs |
| `system-design` | Designing components, boundaries, and data flows |
| `technology-selection` | Comparing frameworks, services, databases, and infrastructure |
| `api-contract-design` | Defining request/response contracts and compatibility |
| `database-schema-design` | Designing entities, relationships, constraints, and indexes |
| `database-migrations` | Planning safe, reversible schema changes |

#### Application development and code quality
| Skill ID | Use it for |
|---|---|
| `python-development` | Python implementation conventions and maintainability |
| `fastapi-development` | FastAPI routes, dependencies, validation, and lifecycle |
| `django-rest-development` | Django REST Framework serializers, views, and permissions |
| `backend-development` | Server-side business logic and service design |
| `frontend-development` | Frontend architecture and implementation |
| `react-development` | React components, hooks, state, and rendering |
| `typescript-development` | Type-safe TypeScript design and implementation |
| `code-quality` | Readability, consistency, and maintainability checks |
| `clean-architecture` | Separating domain, application, and infrastructure concerns |
| `design-patterns` | Applying patterns only where they solve a real problem |
| `refactoring` | Improving structure while preserving behavior |

#### Testing and TDD
| Skill ID | Use it for |
|---|---|
| `test-driven-development` | Red-green-refactor and test-first implementation |
| `unit-testing` | Testing individual functions/classes in isolation |
| `integration-testing` | Testing interactions between modules/services |
| `system-testing` | Testing the complete integrated application |
| `end-to-end-testing` | Testing user journeys through the application |
| `regression-testing` | Ensuring existing behavior does not break |
| `smoke-testing` | Quickly checking critical application paths |
| `sanity-testing` | Narrow checks after a small change or fix |
| `acceptance-testing` | Verifying business acceptance criteria |
| `contract-testing` | Verifying service/API compatibility |
| `performance-testing` | Measuring latency, throughput, and resource use |
| `load-testing` | Testing expected concurrent/traffic levels |
| `stress-testing` | Finding behavior beyond normal capacity |
| `security-testing` | Testing security controls and vulnerability cases |
| `accessibility-testing` | Checking accessibility requirements |
| `test-data-management` | Creating safe, representative, reproducible test data |
| `pytest` | Structuring and running Python tests with pytest |
| `mocking-and-fixtures` | Isolating dependencies and preparing repeatable test setup |

#### Debugging, review, and security
| Skill ID | Use it for |
|---|---|
| `bug-reproduction` | Creating a reliable reproduction before fixing |
| `root-cause-analysis` | Finding the underlying cause rather than symptoms |
| `debugging` | Systematic debugging and hypothesis testing |
| `log-analysis` | Finding useful evidence in application/system logs |
| `code-review` | Reviewing correctness, design, and maintainability |
| `secure-code-review` | Reviewing code for security defects |
| `dependency-auditing` | Checking package health and dependency risks |
| `threat-modeling` | Identifying assets, threats, trust boundaries, and mitigations |
| `authentication-and-authorization` | Reviewing identity, roles, permissions, and access |
| `input-validation` | Validating and normalizing untrusted input |
| `secrets-management` | Handling credentials and sensitive configuration safely |

#### Git, delivery, infrastructure, and operations
| Skill ID | Use it for |
|---|---|
| `git-workflows` | Everyday Git collaboration and safe change management |
| `code-branching` | Choosing and maintaining branch strategies |
| `merge-conflict-resolution` | Resolving conflicts without losing intended changes |
| `ci-cd` | Automating build, test, and delivery pipelines |
| `docker-containerization` | Building and running containerized applications |
| `kubernetes-basics` | Understanding workloads, services, and deployments |
| `jenkins-pipelines` | Structuring Jenkins pipeline stages and agents |
| `release-management` | Preparing and coordinating a release |
| `rollback-planning` | Designing safe recovery from a failed rollout |
| `observability` | Planning logs, metrics, traces, and operational signals |
| `logging-and-metrics` | Adding useful structured logs and service metrics |
| `distributed-tracing` | Following requests across distributed services |
| `incident-response` | Triage, mitigation, communication, and recovery |
| `runbook-authoring` | Writing operational procedures for repeatable incidents |

#### Data engineering and data science
| Skill ID | Use it for |
|---|---|
| `data-ingestion` | Loading data from files, APIs, databases, or streams |
| `data-validation` | Checking schema, constraints, and correctness |
| `data-cleaning` | Handling missing, duplicate, invalid, and inconsistent data |
| `exploratory-data-analysis` | Understanding distributions, patterns, and anomalies |
| `feature-engineering` | Creating model-ready input features |

#### Machine learning and MLOps
| Skill ID | Use it for |
|---|---|
| `ml-experiment-design` | Defining hypotheses, baselines, and controlled experiments |
| `model-training` | Training models reproducibly |
| `transfer-learning` | Adapting a pretrained model to a new task |
| `fine-tuning` | Updating model weights for a target task |
| `hyperparameter-tuning` | Searching model/training configuration space |
| `model-evaluation` | Evaluating model performance and failure cases |
| `classification-metrics` | Accuracy, precision, recall, F1, ROC-AUC, PR-AUC, etc. |
| `regression-metrics` | MAE, MSE, RMSE, R², and related metrics |
| `cross-validation` | Estimating generalization across data splits |
| `data-leakage-prevention` | Preventing train/test contamination and leakage |
| `model-serialization` | Saving/loading models and preprocessing artifacts |
| `model-serving` | Exposing a trained model for inference |
| `batch-inference` | Running inference over offline datasets |
| `online-inference` | Serving low-latency real-time predictions |
| `model-monitoring` | Tracking production quality, latency, and resource use |
| `data-drift-detection` | Detecting changes in input data distributions |
| `concept-drift-detection` | Detecting changes in input-to-target relationships |
| `retraining-strategy` | Defining evidence-based retraining triggers and validation |
| `mlops-pipelines` | Automating data, training, evaluation, and deployment lifecycle |

#### RAG and retrieval engineering
| Skill ID | Use it for |
|---|---|
| `rag-architecture` | Designing the end-to-end retrieval-augmented generation system |
| `document-ingestion` | Loading and normalizing source documents |
| `document-parsing` | Extracting text, tables, layout, and metadata |
| `chunking-strategies` | Choosing chunk sizes, overlap, and semantic boundaries |
| `embedding-model-selection` | Selecting embedding models for corpus/query characteristics |
| `vector-database-design` | Designing vector storage, indexes, and metadata |
| `hybrid-search` | Combining lexical and vector retrieval |
| `metadata-filtering` | Restricting retrieval by structured metadata |
| `reranking` | Reordering retrieved passages with a reranker |
| `retrieval-evaluation` | Measuring whether relevant evidence is retrieved |
| `ragas-evaluation` | Applying Ragas-style RAG evaluation metrics |
| `faithfulness-evaluation` | Checking whether answers are supported by retrieved context |
| `answer-relevance` | Checking whether answers address the user's question |
| `context-precision` | Measuring relevance of retrieved context |
| `context-recall` | Measuring whether relevant context was retrieved |
| `rag-debugging` | Diagnosing ingestion, retrieval, context, and generation failures |

#### LLMs, prompts, and agent engineering
| Skill ID | Use it for |
|---|---|
| `prompt-engineering` | Designing and improving prompts systematically |
| `structured-output` | Producing and validating JSON/schema-constrained output |
| `llm-evaluation` | Comparing LLM output quality and reliability |
| `llm-guardrails` | Adding safety, validation, and policy boundaries |
| `llm-observability` | Tracking model calls, latency, errors, and usage |
| `agent-design` | Defining agent roles, state, boundaries, and responsibilities |
| `tool-design` | Designing safe, narrow, well-described tools |
| `multi-agent-orchestration` | Coordinating agents and specialist responsibilities |
| `agent-handoffs` | Passing context and deliverables between agents |
| `agent-permissions` | Limiting agent access and requiring approvals |
| `model-routing` | Choosing models based on task difficulty and capability |
| `cost-optimization` | Reducing token, provider, and execution cost |
| `latency-optimization` | Reducing end-to-end response time |

#### Documentation and research
| Skill ID | Use it for |
|---|---|
| `technical-documentation` | Writing API, architecture, setup, and maintenance docs |
| `research-with-sources` | Researching technical claims and attaching reliable references |

### How many skills should you load?

Do not load all 119 for every request. Select 1–5 highly relevant skills. Too many instructions
can add noise and consume context. For a complex task, load skills by phase: requirements
first, then implementation/testing, then review/release.

---

## 6. Commands: exact usage

A **command** is a saved prompt template. It is not a shell command and it does not represent
a separate agent. You invoke it inside the OpenCode conversation using `/command-name`.

This package includes four commands:

| Command | Use it when | Main stages |
|---|---|---|
| `/workflow-feature` | Building a new feature | Inspect → requirements → tests first → design → implementation → tests → review |
| `/workflow-bugfix` | Fixing a defect | Reproduce → regression test → root cause → minimal fix → tests → review |
| `/workflow-rag` | Building or evaluating RAG | Define dataset/metrics → inspect pipeline → evaluate retrieval/answers → diagnose → re-evaluate |
| `/workflow-ml` | Training/evaluating/deploying ML | Data audit → baseline → metrics → training → evaluation → monitoring/drift/retraining |

### `/workflow-feature`

Example:

```text
/workflow-feature Add a customer search endpoint with pagination and role-based access.
Use the existing API conventions. Write tests before implementation.
Do not change the database schema unless the design requires it; explain first.
```

Use it for features that cross multiple layers. For a tiny isolated change, ask the developer
agent directly instead.

### `/workflow-bugfix`

Example:

```text
/workflow-bugfix The photo verification endpoint returns 500 when the customer has no
registered face embedding. Reproduce it, add a regression test first, identify the root
cause, make the smallest fix, and run the relevant tests.
```

Use it when the behavior is wrong and you need evidence before patching. Include the exact
error, request, logs, expected behavior, and observed behavior.

### `/workflow-rag`

Example:

```text
/workflow-rag Evaluate my PDF RAG pipeline. Separate retrieval quality from answer quality.
Use a representative question set, measure context precision/recall and faithfulness,
identify failure categories, and recommend the next experiment. Do not change production
code until the evaluation plan is approved.
```

Use it when designing, tuning, or evaluating a RAG pipeline. Do not rely on a single
aggregate score; inspect retrieval misses, unsupported answers, and representative examples.

### `/workflow-ml`

Example:

```text
/workflow-ml Build a reproducible image-classification training and evaluation workflow.
Audit data leakage, establish a baseline, select metrics before training, evaluate by class,
and propose production monitoring and drift triggers.
```

Use it for a model lifecycle task, not only for calling `model.fit()`. Include dataset size,
label quality, target metric, compute constraints, and deployment context.

### How to create another command

Create a Markdown file in `.opencode/commands/`, for example `.opencode/commands/review-diff.md`:

```markdown
---
description: Review the current Git diff for correctness and risk.
agent: code-reviewer
---

Review the current changes.

Check:
1. Correctness and edge cases
2. Security and data handling
3. Performance
4. Test coverage
5. Backward compatibility

Do not modify files. Return findings ordered by severity and cite exact file paths.
```

Then invoke:

```text
/review-diff
```

If the command needs an argument, use the command argument placeholders supported by your
OpenCode version and document the expected syntax in the command. Be careful with shell
expansion in command templates: shell blocks can execute outside the normal agent tool
approval flow, so only use trusted commands.

---

## 7. Tools: what agents use to perform work

A **tool** is an executable capability. Agents reason and decide; tools let them inspect
files, search, edit, run commands, access the web, ask questions, or call external services.

### Key distinction

| Item | What it contributes | Example |
|---|---|---|
| Agent | Role and decision-making instructions | `debugging-agent` |
| Skill | Reusable procedure | `root-cause-analysis` |
| Command | Repeatable prompt/workflow entry point | `/workflow-bugfix` |
| Tool | Executes an action | `grep`, `read`, `bash`, `subagent` |
| MCP server | Adds external tool integrations | Database, Git hosting, issue tracker |
| Custom tool | Your own executable integration | `.opencode/tools/...` |

The package does **not** define 119 custom tools. It relies on OpenCode's built-in tools and
whatever MCP/custom tools you separately configure. A skill may tell an agent how to use a
tool, but it does not create that tool.

### Common built-in tool categories

Names and availability can differ between OpenCode releases. Use the tool list exposed by
your installed version as the source of truth.

| Tool / category | What it does | Use it when | Example request |
|---|---|---|---|
| `read` | Reads a file or file range | Inspect source/config/docs | "Read the router and service files before proposing a change." |
| `glob` | Finds files by path pattern | Discover project structure | "Find all Python test files and all migration files." |
| `grep` | Searches file contents | Find symbols, strings, usages, errors | "Find every call site of `generate_embedding`." |
| `list` | Lists directory contents | Inspect a folder | "List the API package and its subdirectories." |
| `write` | Creates or replaces file content | Create a new file or rewrite a document | "Create a focused regression test file." |
| `edit` | Applies targeted file edits | Make a small modification | "Change only the validation branch in this function." |
| `apply_patch` | Applies a patch/diff | Reviewable multi-line changes | "Apply this minimal patch to the service." |
| `bash` / shell | Runs terminal commands | Tests, Git, package tools, build, lint | "Run `pytest tests/test_auth.py -q`." |
| `subagent` / task | Starts a specialist child agent | Delegate independent analysis or work | "Ask the security reviewer to audit the auth flow." |
| `skill` | Loads a skill's instructions | Apply a reusable procedure | "Load `test-driven-development`." |
| `websearch` | Searches external web sources | Find current documentation or technical facts | "Search the official docs for this library's current API." |
| `webfetch` | Fetches a known web URL | Read a specific documentation page | "Fetch the official migration guide." |
| `question` | Asks the user for a decision | A choice materially affects implementation | "Ask whether this API change may be breaking." |
| `todowrite` / `todoread` | Maintains a task checklist | Multi-step tasks with progress tracking | "Track the migration steps and mark verified tasks." |
| `lsp` | Uses language-server information | Symbol navigation, diagnostics, code intelligence | "Find references and diagnostics for this symbol." |
| `execute` | Runs Code Mode orchestration over available tools | Parallel independent tool calls or structured processing | "Read these three files in parallel and compare their contracts." |
| `browser` | Controls a connected desktop browser, when available | UI testing or browser-based inspection | "Open the local app and inspect the login flow." |
| MCP tools | Calls a connected external service | Issues, source control, databases, monitoring, etc. | "Fetch the linked issue and summarize its acceptance criteria." |
| Custom tools | Runs your own registered functions | A repeatable internal API or controlled operation | "Call the approved deployment-status checker." |

### Effective tool usage

**Read before edit**

```text
Inspect the route, service, schema, and related tests first.
Show me the call path and current behavior. Do not edit yet.
```

**Search before assuming**

```text
Search for all definitions and usages of `verify_customer`.
Include tests, API routes, and callers. Report the exact paths.
```

**Run the narrowest test first**

```text
Run only the test module related to this change. If it passes, run the full relevant suite.
Report the exact commands and output summary.
```

**Use shell commands safely**

Ask for an explanation before approving destructive or broad commands. Review commands such as
`rm`, `git reset --hard`, `git clean`, database drops, migrations, deployment commands, and
commands that transmit secrets or data externally.

### Tool permissions

OpenCode permissions determine whether an agent may use a tool:

- `allow`: execute without asking.
- `ask`: request approval.
- `deny`: tool/action unavailable to that agent.

Use the permission format supported by your installed OpenCode version. Newer OpenCode
documentation may use an action/resource/effect permission model, while older releases and
existing configs may use a different shape. Do not paste a permission example from a different
version without validating it.

A safe default for a team project is:
- Allow normal repository reads.
- Ask before shell commands that modify state or access external systems.
- Ask before edits if you want human review at every change.
- Deny deployment, destructive database operations, and secret exfiltration.
- Give read-only review agents no edit permission.

### MCP and custom tools

MCP servers and custom tools are optional integrations; they are not bundled as configured
connections in this ZIP. Add them only when you need external capabilities such as issue
tracking, Git hosting, database inspection, or deployment status.

Before connecting one:
1. Verify the server/tool publisher and endpoint.
2. Understand what data it can read and what actions it can perform.
3. Grant only the required permissions.
4. Require approval for write, delete, deploy, or financial-impact actions.
5. Never send API keys or private customer data into an untrusted tool.

---

## 8. Model tiers: High, Mid, Small

The tier names are logical categories, not actual model IDs. OpenCode ultimately needs a
real `provider/model` value that exists and is accessible to your account.

| Tier | Best for | Typical agents |
|---|---|---|
| High | Complex reasoning, architecture, hard debugging, security, evaluation, final review | Orchestrator, Architect, Debugging, Security, RAG/ML evaluation |
| Mid | Coding, test design, refactoring, API/database work | Developer, TDD, Test Engineer, Backend, RAG Engineer |
| Small | Classification, simple planning, formatting, routine summaries | Requirements Validator, Project Planner, Documentation, Experiment Tracker |

### How to choose a model

Compare candidate models on your real workload, not only benchmark claims:

- Correctness on your codebase
- Tool-call reliability
- Context-window fit
- Structured output reliability
- Latency
- Cost per completed task
- Provider rate limits and uptime
- Data/privacy requirements

A small model should not make final decisions on high-risk migrations or security findings
without review. A high model is not automatically better for every simple task.

### Configure model IDs

Read `docs/MODEL_SETUP.md`. In this package, each agent Markdown file has a `model` field.
Replace placeholders such as:

```yaml
model: YOUR_MID_PROVIDER/YOUR_MID_MODEL_ID
```

with an actual model ID supported by your provider and installed OpenCode version.

Do not assume the package's `tier` labels dynamically route models. The tier labels are
documentation; the concrete `model` field is what OpenCode uses for the agent.

---

## 9. Recommended end-to-end workflows

### A. Feature development with TDD

1. Run `/workflow-feature`.
2. Ask the Requirements Validator to turn the request into acceptance criteria.
3. Ask the Architect if the change affects system boundaries or data contracts.
4. Ask the TDD Agent to propose tests before implementation.
5. Ask the Developer to implement one small slice.
6. Ask the Test Engineer to run unit/integration/regression checks.
7. Ask the Code Reviewer to inspect the diff.
8. Ask the Acceptance Reviewer to map evidence to each criterion.

Prompt example:

```text
/workflow-feature Add a paginated customer-search endpoint.
Acceptance criteria:
- Requires authenticated access.
- Supports page and page_size with safe limits.
- Returns stable pagination metadata.
- Has tests for empty results, invalid values, and authorization.
Write tests before implementation and preserve existing API compatibility.
```

### B. Bug fixing

1. Run `/workflow-bugfix`.
2. Give the exact error, input, expected behavior, and observed behavior.
3. Reproduce the issue.
4. Write a failing regression test.
5. Trace the root cause.
6. Apply the smallest fix.
7. Run regression and adjacent tests.
8. Review the diff for side effects.

### C. GenAI/RAG evaluation

1. Run `/workflow-rag`.
2. Build a stable evaluation dataset with expected evidence/answers.
3. Evaluate retrieval separately from generation.
4. Categorize errors: ingestion, parsing, chunking, embedding, retrieval, reranking, prompt,
   context truncation, hallucination, or answer formatting.
5. Change one major variable at a time.
6. Compare metrics and examples before/after.
7. Record cost and latency as well as quality.

### D. ML model lifecycle

1. Run `/workflow-ml`.
2. Audit labels, duplicates, class balance, leakage, and train/validation/test split.
3. Establish a baseline and predefine metrics.
4. Train reproducibly with versioned config and data.
5. Evaluate global and slice-level performance.
6. Decide deployment acceptance criteria.
7. Monitor quality, latency, failures, and data/concept drift.
8. Retrain only when evidence and validation justify it; keep rollback available.

### E. CI/CD and release

1. Ask `devops-engineer` to map the current pipeline.
2. Ask `test-engineer` to identify test gates.
3. Ask `security-reviewer` and `dependency-auditor` to review relevant risks.
4. Ask `release-manager` to prepare rollout and rollback.
5. Ask `observability-agent` to verify logs, metrics, and alerts.
6. Require human approval before deployment.

---

## 10. Parallel vs sequential agent execution

### Use sequential execution when

- One phase depends on the previous phase's result.
- Agents need to modify the same files.
- A design decision must be made before implementation.
- A migration or security-sensitive operation needs a controlled sequence.

Example: requirements → design → test → implementation → validation.

### Use parallel execution when

- Tasks are independent.
- Agents can work read-only.
- Their outputs can be integrated without conflicting edits.

Example:

```text
In parallel, have:
1. technical-researcher compare the official library options;
2. security-reviewer inspect the authentication design;
3. test-engineer draft edge-case tests.

Do not edit files. Return separate findings with evidence.
I will choose the design before implementation.
```

Avoid two agents editing the same file simultaneously. Parallel agents can return incompatible
recommendations; the orchestrator must reconcile them.

---

## 11. Failure handling and retry discipline

The agent instructions recommend a bounded approach:

1. Classify the failure: provider/network/tool failure, permission denial, code defect, test
   failure, missing dependency, or ambiguous requirement.
2. Retry a transient provider/tool failure only a small number of times.
3. Do not repeat the same failed command without changing the cause.
4. For a difficult reasoning task, delegate to a stronger specialist/model if configured.
5. For code defects, use logs and tests rather than repeatedly asking the model to guess.
6. Stop for human approval before destructive or production-impacting operations.
7. If still blocked, return exact evidence and the next smallest action.

**Important:** these rules are prompt instructions. They do not guarantee automatic retry,
automatic model escalation, or durable recovery. OpenCode's runtime and configured tools
determine the actual behavior.

---

## 12. How to add your own agent

The quickest supported approach is to use OpenCode's agent creation command if available
in your installed version:

```bash
opencode agent create
```

Or create a Markdown file under `.opencode/agents/`, for example
`.opencode/agents/api-contract-reviewer.md`:

```markdown
---
description: Reviews API contracts for compatibility, validation, and error semantics.
mode: subagent
model: YOUR_MID_PROVIDER/YOUR_MID_MODEL_ID
---

# API Contract Reviewer

You review API contracts without changing application files.

Check:
- HTTP methods and status codes
- Request and response schemas
- Authentication and authorization
- Validation and error formats
- Pagination and filtering
- Backward compatibility
- Contract tests

Return findings ordered by severity, with exact file paths and evidence.
Do not claim tests were run unless you actually ran them.
```

Use the exact frontmatter keys supported by your installed OpenCode version. Then invoke the
agent from the primary session, for example:

```text
@api-contract-reviewer Review the customer API contract. Do not edit files.
```

---

## 13. How to add your own skill

Create a directory and a `SKILL.md`:

```text
.opencode/skills/api-versioning/
└── SKILL.md
```

Example:

```markdown
---
name: api-versioning
description: Plan backward-compatible API versioning and deprecation.
---

# API Versioning

Use this skill when adding, changing, or deprecating public API behavior.

## Procedure
1. Identify existing clients and supported versions.
2. Classify the change as additive, behavior-changing, or breaking.
3. Prefer additive changes where practical.
4. Define compatibility and deprecation policy.
5. Update schemas, examples, and contract tests.
6. Validate old and new client behavior.
7. Document rollout and removal milestones.

## Quality checklist
- [ ] Existing clients remain compatible or migration is explicit.
- [ ] Contract tests cover supported versions.
- [ ] Deprecation behavior is documented.
- [ ] Rollback path is understood.
```

The directory name `api-versioning` is the skill ID. Ask the agent to use it:

```text
Use @api-versioning before changing the public API.
First report whether the requested change is breaking.
```

Keep a skill focused. One skill should teach one coherent procedure rather than trying to
contain the entire software development lifecycle.

---

## 14. How to add a custom tool

Custom tools are executable functions, unlike skills. OpenCode supports project-local custom
tool definitions in `.opencode/tools/` (check the exact API and package version in the
official docs before implementing).

Use a custom tool when the agent needs a narrow, repeatable operation that cannot be safely
or conveniently expressed through existing built-in tools.

Good custom tool examples:
- Read-only deployment status
- Query a specific internal service with validated parameters
- Fetch a ticket by ID
- Run an approved test suite and return structured results

Bad custom tool examples:
- An unrestricted shell wrapper
- A tool that silently deletes or deploys
- A tool that returns secrets to the model
- A generic database query tool with unrestricted write access

Design principles:
1. One tool should do one clearly described operation.
2. Validate every argument.
3. Use least-privilege credentials.
4. Set timeouts and bounded output.
5. Return structured results and actionable errors.
6. Log operations without logging secrets.
7. Require explicit confirmation for irreversible actions.

A skill can explain when to call the tool; the tool implementation performs the action.

---

## 15. Troubleshooting

| Symptom | Likely cause | What to check |
|---|---|---|
| Agent does not appear | Wrong path, invalid frontmatter, unsupported config key | Confirm `.opencode/agents/<id>.md`, inspect OpenCode diagnostics |
| Agent appears but uses wrong model | Placeholder or invalid `provider/model` | Run model listing for your OpenCode version; verify provider/model ID |
| Agent cannot delegate | Subagent permission or nesting depth restriction | Check parent permission and subagent depth settings |
| Skill is not discovered | Wrong path/name, missing description, skill permission denied | Confirm `.opencode/skills/<id>/SKILL.md` and exact ID |
| Skill is not loaded automatically | Agent judged it irrelevant or it is not advertised | Explicitly mention `@skill-id` |
| Slash command is missing | Wrong directory, filename, or command discovery cache/version | Confirm `.opencode/commands/<command>.md` and restart/reload |
| Shell command is blocked | Bash/shell permission requires approval or is denied | Review the prompt and permission config |
| Agent edits when you expected read-only | Edit permission is too broad | Restrict edit/write/patch permissions for that agent |
| Model/provider errors | Invalid credentials, endpoint, model ID, rate limit, incompatible API | Verify provider setup, environment variables, and model availability |
| Two agents produce conflicting edits | Parallel work overlapped | Separate file ownership and have one integration agent merge |
| Agent repeats a failed step | Prompt-level retry behavior is insufficient | Give exact failure evidence, limit attempts, change the hypothesis |
| Tests pass locally but fail in CI | Environment/configuration mismatch | Compare runtime, dependencies, env vars, test data, and CI commands |

Useful terminal checks (commands can vary by OpenCode version):

```bash
opencode --version
opencode models
opencode agent --help
```

Use `opencode --help` to discover commands supported by your installed release. Do not
assume a command exists just because a different release documents it.

---

## 16. Practical rules for getting high-quality results

1. **Give repository context.** Mention the service, module, endpoint, file, or error.
2. **Set the action boundary.** Say “investigate only”, “plan only”, or “you may edit”.
3. **Ask for evidence.** Request exact file paths, symbols, test names, command outputs, and
   sources. Never accept invented line numbers or fabricated test results.
4. **Define acceptance criteria.** “Make it better” is not testable.
5. **Use TDD for behavior changes.** Reproduce a bug with a failing test before the fix when
   practical.
6. **Keep changes small.** One feature slice or root cause at a time.
7. **Use independent review.** Ask a separate reviewer to inspect important changes.
8. **Measure ML/LLM changes.** Compare against a baseline and inspect examples, not only one
   aggregate metric.
9. **Control permissions.** Do not give every agent unrestricted shell, network, and write
   access by default.
10. **Require a final evidence report.** Files changed, tests run, outcomes, known limitations,
    and next steps.

### Reusable prompt template

```text
Objective:
[What must be achieved]

Context:
[Relevant files, endpoint, logs, stack, constraints]

Mode:
[Investigate only / plan only / implement / review]

Scope:
[Allowed files and actions; prohibited actions]

Acceptance criteria:
1. [...]
2. [...]
3. [...]

Required workflow:
[Agents and skills to use, tests to run, review needed]

Final response:
- Findings and decisions
- Files changed
- Tests/commands actually run and results
- Risks and limitations
- Follow-up actions
```

---

## 17. Official references

OpenCode changes over time. Check the official documentation for your installed version
before changing configuration syntax or relying on a particular tool.

- Agents: https://opencode.ai/docs/agents/
- Skills: https://opencode.ai/docs/skills/
- Commands: https://opencode.ai/docs/commands/
- Tools: https://opencode.ai/docs/tools/
- Permissions: https://opencode.ai/docs/permissions/
- Custom tools: https://opencode.ai/docs/custom-tools/
- Configuration: https://opencode.ai/docs/config/

---

## 18. Final quick reference

| If you want to… | Use |
|---|---|
| Build a feature end-to-end | `/workflow-feature` |
| Fix a reproducible defect | `/workflow-bugfix` |
| Evaluate a RAG system | `/workflow-rag` |
| Train/evaluate/monitor an ML model | `/workflow-ml` |
| Understand requirements | `requirements-validator` + `requirements-discovery` |
| Design architecture | `solution-architect` + `system-design` |
| Choose a framework/database/model | `technology-selector` + `technology-selection` |
| Write tests first | `tdd-agent` + `test-driven-development` |
| Find a root cause | `debugging-agent` + `root-cause-analysis` |
| Review a diff | `code-reviewer` + `code-review` |
| Audit security | `security-reviewer` + `threat-modeling` + `secure-code-review` |
| Tune performance | `performance-engineer` + `performance-testing` |
| Build CI/CD | `devops-engineer` + `ci-cd` |
| Design model lifecycle | `ml-architect` + ML skills |
| Improve RAG retrieval | `rag-engineer` + retrieval skills |
| Measure RAG quality | `rag-evaluation-agent` + `ragas-evaluation` |
| Design agent tools | `agent-engineer` + `tool-design` |
| Add reusable knowledge | Create `.opencode/skills/<id>/SKILL.md` |
| Add a specialist | Create `.opencode/agents/<id>.md` |
| Add a repeatable workflow | Create `.opencode/commands/<name>.md` |
| Add an executable integration | Create/configure a custom tool or MCP server |

**The effective pattern is:** one orchestrator, a small number of relevant specialists,
only the skills needed for the task, the minimum necessary tools and permissions, and
verification based on real evidence.
