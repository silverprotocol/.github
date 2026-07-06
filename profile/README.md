<p align="center">
  <img src="./hero.png" width="240" alt="A glass prism splitting a beam of white light into a rainbow" />
</p>

<h1 align="center">Silver Protocol</h1>
<p align="center"><b>AgJSON</b> — the open, neutral, typed transport for normalized agent-framework I/O.</p>

<p align="center">
  <a href="https://www.npmjs.com/package/@silverprotocol/core"><img src="https://img.shields.io/npm/v/%40silverprotocol%2Fcore?label=%40silverprotocol%2Fcore&color=0a7"></a>
  <a href="https://github.com/silverprotocol/typescript-sdk/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue"></a>
  <a href="https://silverprotocol.io/AgJSON"><img src="https://img.shields.io/badge/spec-1.0.0--draft.1-orange"></a>
</p>

---

Every agent framework — Claude Agent SDK, OpenAI Agents SDK, Google ADK, LangGraph,
Vercel AI SDK — streams its own shape of events. If you're building a client, a UI,
or a tool that needs to work across more than one of them, you end up writing (and
maintaining) a bespoke adapter per framework.

**AgJSON is the wire format that ends that.** It normalizes any framework's native
event stream into one typed, versioned, forward-compatible shape — so a client
written against AgJSON works with every framework a normalizer exists for, today
and after the next SDK release.

- **Typed, not `any`.** Every event and block is a discriminated union member —
  no `Record<string, unknown>` escape hatches for shaped data.
- **Streaming-first.** `push(native): AgEvent[]` / `flush(): AgEvent[]` — a
  stateful, per-invoke normalizer that folds a live stream into a persisted
  `AgReduceResult` via the normative `reduce()`.
- **Respects the UI layer.** AgJSON carries content and interaction anchors
  (Layer-A); rendering is left to **MCP Apps** and **A2UI** — no reinvented
  component schema.
- **Forward-compatible by rule.** Unknown event types and unknown fields are
  always safe to ignore, so a v1.0 client keeps working as the spec grows.

## Get started

```bash
npm install @silverprotocol/core @silverprotocol/claude-agent-sdk
# or @silverprotocol/openai-agents / @silverprotocol/google-adk for other frameworks
```

```ts
import { createClaudeAgentSdkNormalizer } from "@silverprotocol/claude-agent-sdk";

const normalizer = createClaudeAgentSdkNormalizer();
for await (const nativeEvent of stream) {
  for (const agEvent of normalizer.push(nativeEvent)) {
    send(agEvent); // one shape, any framework on the other end
  }
}
```

## Repositories

| Repo | What's in it |
| --- | --- |
| [`typescript-sdk`](https://github.com/silverprotocol/typescript-sdk) | The AgJSON spec (`SPEC.md`) + the reference TypeScript SDK: `core` and per-framework normalizers (`claude-agent-sdk`, `openai-agents`, `google-adk`). Start here. |

More language SDKs land the same way: subtree-vendored from the private spec
workspace, spec and code in lockstep.

## Status

AgJSON is **Draft** (wire version `1.0.0-draft.1`) — the shape is stable enough
to build on, and we want your framework's edge cases before the v1 freeze.
Issues and PRs on [`typescript-sdk`](https://github.com/silverprotocol/typescript-sdk)
are the fastest way to shape what ships.

**Spec:** [silverprotocol.io/AgJSON](https://silverprotocol.io/AgJSON) · **License:** MIT

<sub>Hero image © Encyclopædia Britannica, Inc., used under [CC BY 4.0](https://www.britannica.com/bps/license/creative-commons-legal-code/623330).</sub>
