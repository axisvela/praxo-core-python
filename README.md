# PRAXO-CORE for Python

PRAXO-CORE is a dependency-free Python framework for building tool-using AI agents. It provides the agent runtime, not a model or hosted service, so you keep control of your models, tools, storage, and sandboxing.

## At a glance

| Field | Value |
| --- | --- |
| Package | `praxo-core` |
| Registry | [PyPI](https://pypi.org/project/praxo-core/0.10.0/) |
| Install | `pip install praxo-core==0.10.0` |
| Source version | `0.10.0` |
| Published version | `0.10.0` |
| Source tag | `python/v0.10.0` |
| Tutorial tag | `python/tutorial/v0.1.0` |
| Examples tag | `python/examples/v0.1.0` |
| Tier | Core |
| Contract | `SPEC_VERSION` `0.2.0` |
| Documentation | https://axisvela.github.io/praxo-core/languages/python/ |
| License | MIT |

## Highlights

- Async-first models, tools, events, and sessions.
- Policy hooks, approvals, path checks, PII redaction, and call limits.
- Context budgets, checkpoints, middleware, and fallback models.
- Streaming, cancellation, and OpenAI-compatible, Anthropic, and Gemini adapters.
- No runtime dependencies; Python 3.11 or newer.

## Quick start

Install the published package:

```bash
pip install praxo-core==0.10.0
```

```python
import asyncio

from praxo.agent import Agent, AgentOptions, Session
from praxo.event import Event
from praxo.model import Message, Response, Usage
from praxo.tool import FuncTool, Registry


class EchoModel:
    async def generate(self, request):
        return Response(message=Message(role="assistant", content="hello"))


class LogSink:
    async def emit(self, event: Event) -> None:
        print(event.to_wire())


async def main() -> None:
    tools = Registry()
    tools.register(FuncTool("echo", "Echo input", lambda args: args))
    agent = Agent(
        AgentOptions(model=EchoModel(), tools=tools, events=LogSink())
    )
    result = await agent.run(Session(id="session-1"), "hi")
    print(result.output, result.steps, result.usage)


asyncio.run(main())
```

See the [full Python README](language/python/README.md) for complete installation, build, and verification commands.

## What's included

| Capability | Status | Detail |
| --- | --- | --- |
| OpenAI-compatible model | native | `praxo.adapters.openaicompat` |
| Anthropic model | native | `praxo.adapters.anthropic.AnthropicModel`; SSE streaming reassembly |
| Gemini model | native | `praxo.adapters.gemini.GeminiModel`; SSE streaming |
| MCP client | native | `praxo.adapters.mcp.MCPClient`; stdio, plus `mcp_http.StreamableHTTPTransport` for Streamable HTTP 2025-03-26 |
| Harmovela sink | native | `praxo.adapters.harmovela.HarmovelaSink` |
| OpenTelemetry sink | native (transport seam) | `praxo.adapters.otel.OtelSink`; caller supplies OTLP/HTTP transport |
| Subagent delegation | native | `praxo.delegation.SubagentManager` |
| Retriever | native | `praxo.retriever.Document` and `Retriever` |

## Examples

Runnable examples live under [`language/python/examples`](language/python/examples).

| Example | What it shows |
| --- | --- |
| [advanced-delegation](language/python/examples/advanced-delegation) | This example shows an application-level parent/child agent pattern. The parent agent may call a `delegate` tool but not the child's `record` tool. The delegate tool runs a child... |
| [checkpoint](language/python/examples/checkpoint) | This example uses published PyPI `praxo-core==0.9.0` to keep checkpoint storage application-owned while using the SDK allowlist for payload encoding. |
| [consumer](language/python/examples/consumer) | This starter installs `praxo-core==0.10.0` from PyPI in a dedicated virtual environment. It does not import the repository's Python implementation. Use Python 3.11 or newer. No... |
| [event-sink](language/python/examples/event-sink) | This example uses published PyPI `praxo-core==0.9.0` to observe agent lifecycle and tool events without coupling the agent to a logging or telemetry backend. |
| [hitl](language/python/examples/hitl) | This example demonstrates a write tool that requires human approval: |
| [identity-policy](language/python/examples/identity-policy) | This example uses published PyPI `praxo-core==0.9.0` to bind a trusted principal to Core policy authorization. The application owns authentication and principal storage; Core on... |
| [live-model](language/python/examples/live-model) | This example uses published PyPI `praxo-core==0.9.0` to connect the Python Core to an OpenAI-compatible endpoint and registers one low-risk `get_time` tool. |
| [mcp](language/python/examples/mcp) | This example uses published PyPI `praxo-core==0.9.0` to connect to a minimal stdio MCP server, list its tools, register them in a Core registry, and call one tool. |
| [provider-fallback](language/python/examples/provider-fallback) | This example uses published PyPI `praxo-core==0.9.0` to fall back to a second model when the primary model call fails. |
| [representative-chain](language/python/examples/representative-chain) | This deterministic example exercises the same application seams as the Go representative chain without a live provider: |
| [retry](language/python/examples/retry) | This example uses published PyPI `praxo-core==0.9.0` to retry transient model failures without changing the agent loop. |
| [service](language/python/examples/service) | This example embeds published PyPI `praxo-core==0.9.0` in a standard-library HTTP service. The application supplies a principal and tenant, restricts tools, and returns JSON. |
| [service-auth](language/python/examples/service-auth) | This example embeds published PyPI `praxo-core==0.9.0` in a standard-library HTTP service with application-owned identity, tenant policy, an optional external policy decision se... |
| [session-checkpoint](language/python/examples/session-checkpoint) | This example saves and restores a conversation history across two agent turns. It uses a teaching in-memory store; production storage and retention remain application-owned. |
| [streaming](language/python/examples/streaming) | This example uses published PyPI `praxo-core==0.9.0` to consume deltas while the agent loop keeps running normally. |
| [structured-output](language/python/examples/structured-output) | This example uses published PyPI `praxo-core==0.9.0` to turn free text into validated JSON with `StructuredOutput`. |
| [structured-output-app](language/python/examples/structured-output-app) | This example turns a directory of text files into validated JSON lines with `StructuredOutput`. It is an application-shaped path: input files, a validated result file, a separat... |

## Scenario Tutorials

Scenario-oriented tutorials live under `language/python/Tutorial`.

| Path | What it shows |
| --- | --- |
| [Tutorial](language/python/Tutorial) | ﻿# Python Tutorials |
| [audiobook-production](language/python/Tutorial/audiobook-production) | Plans fixed licensed voices and mixing cues and drafts a review-only plan. No synthesis, rendering or rights guarantee. |
| [biodiversity-alerts](language/python/Tutorial/biodiversity-alerts) | A bounded first slice of [AE-05](https://axisvela.github.io/praxo-core/getting-started/agent-scenarios/), not an anti-poaching dispatch system. It classifies fixed labeled obser... |
| [bug-repair](language/python/Tutorial/bug-repair) | A bounded first slice of [SE-02](https://axisvela.github.io/praxo-core/getting-started/agent-scenarios/), not an autonomous coding agent. It reproduces a seeded defect, applies... |
| [cbt-exercise](language/python/Tutorial/cbt-exercise) | A bounded first slice of [HC-05](https://axisvela.github.io/praxo-core/getting-started/agent-scenarios/), not a therapist or crisis service. It walks a fixed synthetic CBT exerc... |
| [clinical-guidelines](language/python/Tutorial/clinical-guidelines) | A bounded first slice of [HC-01](https://axisvela.github.io/praxo-core/getting-started/agent-scenarios/), not diagnosis or treatment advice. It retrieves fixed synthetic guideli... |
| [credit-diligence](language/python/Tutorial/credit-diligence) | A bounded first slice of [FC-03](https://axisvela.github.io/praxo-core/getting-started/agent-scenarios/), not a credit decision or fraud adjudication system. It computes fixed r... |
| [dubbing-plan](language/python/Tutorial/dubbing-plan) | A bounded first slice of [MD-04](https://axisvela.github.io/praxo-core/getting-started/agent-scenarios/), not a dubbing or voice-cloning product. It reads fixed licensed media s... |
| [education-tutoring](language/python/Tutorial/education-tutoring) | A native Python Core tutorial for a guided study plan. An application-owned `LessonRetriever` implements the Core `Retriever` protocol and returns typed `Document`s with metadat... |
| [insurance-claims](language/python/Tutorial/insurance-claims) | A native Python Core tutorial for review-only claim triage. Two fixed tools read a synthetic claim file and draft a decision; the deterministic model packages the tool-computed... |
| [legal-compliance](language/python/Tutorial/legal-compliance) | A native Python Core tutorial for a compliance memo with a human-review gate. One tool searches fixed policy fixtures; a second tool publishes the memo only after the applicatio... |
| [literature-review](language/python/Tutorial/literature-review) | Retrieves two fixed abstracts and drafts unverified hypotheses with citations. No fabricated papers or verified conclusions. |
| [litigation-timeline](language/python/Tutorial/litigation-timeline) | Builds a cited timeline from fixed documents and drafts a review-only examination outline. No legal advice or admissibility claim. |
| [marketing-segments](language/python/Tutorial/marketing-segments) | Computes fixed consent-aware segments and drafts review-only content. No publication or tracking. |
| [molecule-screen](language/python/Tutorial/molecule-screen) | A bounded first slice of [SC-02](https://axisvela.github.io/praxo-core/getting-started/agent-scenarios/), not drug discovery. It ranks fixed synthetic molecular descriptors unde... |
| [negotiation-interpretation](language/python/Tutorial/negotiation-interpretation) | Compares fixed transcript terms, flags uncertainty and drafts review-only notes. No mind reading, micro-expression inference or live interpretation guarantee. |
| [office-qa](language/python/Tutorial/office-qa) | A native Python Core tutorial for office-policy Q&A with citations. The Agent has one fixed tool, `search_documents`, which searches three synthetic policy documents. The determ... |
| [order-review](language/python/Tutorial/order-review) | First local integration slice, not a completed HR or Web tutorial. The native Python Core chooses a scoped `review_order` tool. The tool reads a synthetic order through HTTP and... |
| [patent-fto](language/python/Tutorial/patent-fto) | A bounded first slice of [LJ-02](https://axisvela.github.io/praxo-core/getting-started/agent-scenarios/), not a freedom-to-operate or patentability opinion. It compares fixed sy... |
| [pentest-lab-review](language/python/Tutorial/pentest-lab-review) | Inventories one fixed loopback lab and drafts a review-only report. No external scanning or exploit chain. |
| [phishing-awareness](language/python/Tutorial/phishing-awareness) | A bounded first slice of [CY-04](https://axisvela.github.io/praxo-core/getting-started/agent-scenarios/), not a mail-security product. It analyzes one fixed synthetic email and... |
| [pollution-tracing](language/python/Tutorial/pollution-tracing) | A bounded first slice of [CT-05](https://axisvela.github.io/praxo-core/getting-started/agent-scenarios/), not an environmental enforcement system. It compares fixed sensor and w... |
| [precedent-outline](language/python/Tutorial/precedent-outline) | A bounded first slice of [LJ-04](https://axisvela.github.io/praxo-core/getting-started/agent-scenarios/), not legal research or advice. It retrieves fixed synthetic precedents a... |
| [recruiting-match](language/python/Tutorial/recruiting-match) | A bounded first slice of [OP-05](https://axisvela.github.io/praxo-core/getting-started/agent-scenarios/), not an automated hiring system. It matches fixed synthetic candidates a... |
| [threat-hunting](language/python/Tutorial/threat-hunting) | Searches fixed logs and drafts a cited synthetic TTP timeline. No external intelligence or APT detection guarantee. |
| [trial-screening](language/python/Tutorial/trial-screening) | A bounded first slice of [HC-03](https://axisvela.github.io/praxo-core/getting-started/agent-scenarios/), not patient recruitment or eligibility adjudication. It evaluates fixed... |
| [virtual-pi](language/python/Tutorial/virtual-pi) | A bounded first slice of [OG-07](https://axisvela.github.io/praxo-core/getting-started/agent-scenarios/), not an autonomous research team. It coordinates three fixed literature/... |

## Verify

This mirror contains the language source and the shared `spec/` vectors, so the implementation can be built and tested here. See [`language/python/README.md`](language/python/README.md) for the exact commands. The monorepo CI runs conformance, API snapshot, and native tests for this implementation.

## Community

- X: <https://x.com/axisvela>
- Weibo: <https://weibo.com/u/1936025761>
- LinkedIn: <https://www.linkedin.com/company/axisvela/>
- Facebook: <https://www.facebook.com/profile.php?id=61596905732406>
- YouTube: <https://www.youtube.com/@axisvela>
- Discord: <https://discord.com/channels/1575163993767809064/1575164343148875977>
- GitHub Discussions: <https://github.com/axisvela/praxo-core/discussions>
- GitHub Issues: <https://github.com/axisvela/praxo-core/issues>
- Documentation: <https://axisvela.github.io/praxo-core/>
- Architect: <https://axisvela.architect.pub>

## Repository layout

- `language/python/` — implementation source, tests, API snapshot, and examples
- `spec/` — shared contract, vectors, and parity inventories
- `LICENSE` — MIT license

## Canonical source

- Canonical source: https://github.com/axisvela/praxo-core/tree/main/language/python
- Registry introduction: `language/python/REGISTRY.md`
- Machine-readable entry: `language/python/llms.txt` and `language/python/llms.md`
- Mirror repository: https://github.com/axisvela/praxo-core-python
- Release index: https://axisvela.github.io/praxo-core/languages/registry-status/

This repository is a discovery mirror of the canonical Python source. The monorepo remains the source of truth and the release workflow source.

PRAXO-CORE is not a hosted model, workflow engine, identity provider, or safety controller. Do not infer exactly-once execution, complete sandbox isolation, production security certification, live-provider behavior, or physical safety from this package.

MIT licensed.
