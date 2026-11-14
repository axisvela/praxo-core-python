# PRAXO-CORE for Python

PRAXO-CORE is a dependency-free Python framework for building tool-using AI agents. It provides the agent runtime, not a model or hosted service, so you keep control of your models, tools, storage, and sandboxing.

## At a glance

| Field | Value |
| --- | --- |
| Package | `praxo-core` |
| Registry | [PyPI](https://pypi.org/project/praxo-core/0.7.0/) |
| Install | `pip install praxo-core==0.7.0` |
| Source version | `0.7.0` |
| Published version | `0.7.0` |
| Source tag | `python/v0.7.0` |
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
pip install praxo-core==0.7.0
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
| Anthropic model | native | `praxo.adapters.anthropic.AnthropicModel` |
| Gemini model | native | `praxo.adapters.gemini.GeminiModel` |
| MCP client | native | `praxo.adapters.mcp.MCPClient`; stdio, plus `mcp_http.StreamableHTTPTransport` for Streamable HTTP 2025-03-26 |
| Harmovela sink | native | `praxo.adapters.harmovela.HarmovelaSink` |
| OpenTelemetry sink | native (transport seam) | `praxo.adapters.otel.OtelSink`; caller supplies OTLP/HTTP transport |
| Subagent delegation | native | `praxo.delegation.SubagentManager` |
| Retriever | native | `praxo.retriever.Document` and `Retriever` |

## Examples

Runnable examples live under [`language/python/examples`](language/python/examples).

| Example | What it shows |
| --- | --- |
| [checkpoint](language/python/examples/checkpoint) | This example uses published PyPI `praxo-core==0.6.0` to keep checkpoint storage application-owned while using the SDK allowlist for payload encoding. |
| [consumer](language/python/examples/consumer) | This starter installs `praxo-core==0.7.0` from PyPI in a dedicated virtual environment. It does not import the repository's Python implementation. Use Python 3.11 or newer. No r... |
| [event-sink](language/python/examples/event-sink) | This example uses published PyPI `praxo-core==0.6.0` to observe agent lifecycle and tool events without coupling the agent to a logging or telemetry backend. |
| [hitl](language/python/examples/hitl) | This example demonstrates a write tool that requires human approval: |
| [identity-policy](language/python/examples/identity-policy) | This example uses published PyPI `praxo-core==0.6.0` to bind a trusted principal to Core policy authorization. The application owns authentication and principal storage; Core on... |
| [live-model](language/python/examples/live-model) |  |
| [mcp](language/python/examples/mcp) | This example uses published PyPI `praxo-core==0.6.0` to connect to a minimal stdio MCP server, list its tools, register them in a Core registry, and call one tool. |
| [provider-fallback](language/python/examples/provider-fallback) | This example uses published PyPI `praxo-core==0.6.0` to fall back to a second model when the primary model call fails. |
| [retry](language/python/examples/retry) | This example uses published PyPI `praxo-core==0.6.0` to retry transient model failures without changing the agent loop. |
| [service](language/python/examples/service) | This example embeds published PyPI `praxo-core==0.6.0` in a standard-library HTTP service. The application supplies a principal and tenant, restricts tools, and returns JSON. |
| [session-checkpoint](language/python/examples/session-checkpoint) | This example saves and restores a conversation history across two agent turns. It uses a teaching in-memory store; production storage and retention remain application-owned. |
| [streaming](language/python/examples/streaming) | This example uses published PyPI `praxo-core==0.6.0` to consume deltas while the agent loop keeps running normally. |
| [structured-output](language/python/examples/structured-output) | This example uses published PyPI `praxo-core==0.6.0` to turn free text into validated JSON with `StructuredOutput`. |

## Verify

This mirror contains the language source and the shared `spec/` vectors, so the implementation can be built and tested here. See [`language/python/README.md`](language/python/README.md) for the exact commands. The monorepo CI runs conformance, API snapshot, and native tests for this implementation.

## Community

- X: <https://x.com/axisvela>
- Weibo: <https://weibo.com/u/1936025761>

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
