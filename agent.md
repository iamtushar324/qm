# Personal fork context

Read upstream `AGENTS.md` and this file before changing this fork.

The objective is a personal QM installation that can absorb upstream changes with
minimal maintenance, followed by safe use of an existing two-node Kubernetes lab.
The user requested installation first, then staged Kubernetes improvements.

The original architecture assessment identified durable PostgreSQL queues, leases,
cross-instance signals, and separate worker entrypoints as useful foundations.
It did not prove failover safety. In particular, a worker losing database access may
continue external actions while its lease expires. Database fencing alone cannot
stop those external effects. Treat network-partition recovery, workspace exclusivity,
and ambiguous command retries as explicit test cases.

Scope and boundaries:

- Personal infrastructure only. Do not import employer repositories, credentials,
  private session transcripts, or deployment material into this public fork.
- Preserve upstream behavior whenever configuration or an external overlay suffices.
- Keep `cloud.md` current with the deployment/update contract. The private operations
  repository is authoritative for actual hostnames, secret locations, and live state.
- The selected provider is the existing personal Codex subscription, imported into
  QM's encrypted keychain. Keep deployment secrets outside Git.
- A healthy web page is not a working installation: prove authenticated chat, a real
  provider response, persisted history, and a sandbox file read independently.
- Explain each infrastructure checkpoint, inspect its result, and record rollback.
- Never claim uninterrupted process migration or exactly-once external actions.
- Source updates use merge ancestry and an independent review before `main`.

At fork initialization, upstream baseline was
`8ac53f7a523337266f5628406d30a23b71c2a2ff`. The first deployment candidate is upstream
release `v0.1.12`, source `5a5cb51260b13000dda5d890d40c877c88d87555`, using its
published immutable images. This difference is intentional and must remain visible
in the private release lock. No application-code modifications are part of this setup.
