# Tools and Integrations Catalogue

This catalogue is advisory. Tools are not installed by these Markdown files. Verify availability, versions, licenses, credentials, permissions and security before enabling them.

## OpenCode-native capabilities
| Capability | Use |
|---|---|
| Agents | Primary and subagent instruction files |
| Skills | On-demand reusable procedures in `SKILL.md` |
| Commands | Reusable slash-command prompts |
| Shell | Run approved local commands |
| Read/Edit/Glob/Grep | Inspect and modify repository files |
| MCP | Connect external tools when configured |

## Research and planning
- Web search/browser, GitHub, Context7, official documentation, package registries.
- Mermaid, PlantUML, Jira, GitHub Projects, Confluence.

## Development and quality
- Git; Python/Node/Java toolchains; pytest, unittest, Vitest, Jest, JUnit.
- Ruff, Mypy, Pylint, SonarQube, Semgrep.
- pdb, debugpy, Sentry.

## Testing
- Playwright, Selenium, Postman, REST Assured.
- k6, Locust, JMeter, Lighthouse, axe-core.
- OWASP ZAP, Burp Suite, Checkmarx, Trivy, Stryker, Allure, TestRail.

## DevOps and operations
- Jenkins, GitHub Actions, GitLab CI, Docker, Podman, Kubernetes, Helm, Quay, Argo CD.
- Prometheus, Grafana, OpenTelemetry, ELK, Loki, PagerDuty.

## Data and ML
- Pandas, Polars, NumPy, Spark, Kafka, Airflow, Prefect.
- Great Expectations, Pandera, DVC, Label Studio.
- Scikit-learn, PyTorch, TensorFlow/Keras, XGBoost, LightGBM, Optuna.
- MLflow, Weights & Biases, Kubeflow, ZenML, Feast.

## GenAI and RAG
- Hugging Face Transformers/Hub, LiteLLM, OpenAI-compatible SDKs, NVIDIA/Groq SDKs, Ollama, vLLM.
- LangChain, LangGraph, LlamaIndex, Docling, DSPy, MCP SDK.
- PostgreSQL/pgvector, FAISS, Qdrant, Milvus, Elasticsearch.
- Ragas, DeepEval, TruLens, LangSmith, Arize Phoenix, Promptfoo, Garak, PyRIT, Giskard.

## Model optimization and serving
- ONNX Runtime, TensorRT, bitsandbytes, llama.cpp, NVIDIA Nsight.
- BentoML, NVIDIA Triton, FastAPI, Docker and Kubernetes.

## Integration policy
1. Prefer existing project tools and official SDKs.
2. Pin dependencies and record versions.
3. Use least-privilege credentials.
4. Keep secrets in environment/secret managers, never Markdown.
5. Use deterministic tools for exact operations; use LLMs for reasoning and interpretation.
