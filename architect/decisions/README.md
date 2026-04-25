# Architecture Decision Records

This folder contains ADRs for the health system. Each ADR captures a key design decision: the context that led to it, the decision itself, and the consequences (both positive and negative).

ADRs are written when a decision is made, not before. They are never deleted — if a decision is reversed, the original ADR is marked `Superseded` and a new ADR is written explaining why.

## Index

| ADR | Title | Date | Status |
|-----|-------|------|--------|
| [ADR-001](ADR-001-icloud-as-file-system.md) | Use iCloud Drive as the system file store | 2026-04-06 | Accepted |
| [ADR-002](ADR-002-tiered-data-model.md) | Tiered data model — hot / warm / cold | 2026-04-06 | Accepted |
| [ADR-003](ADR-003-agent-isolation.md) | Agent isolation — each agent runs in its own clean session | 2026-04-06 | Accepted |
| [ADR-004](ADR-004-single-writer-per-state.md) | Single Writer Per State | 2026-04-09 | Accepted |
| [ADR-005](ADR-005-sessions-have-no-reliable-endings.md) | Design for sessions with no reliable endings | 2026-04-10 | Accepted |
| [ADR-006](ADR-006-lane-discipline-system-status.md) | Lane discipline — extract infrastructure state into system_status.md | 2026-04-16 | Accepted |
| [ADR-007](ADR-007-flora-manual-entry.md) | Flora manual entry — design decisions | 2026-04-16 | Accepted |
| [ADR-008](ADR-008-prompt-versioning.md) | Prompt versioning standard | 2026-04-11 | Accepted |
| [ADR-009](ADR-009-prim-as-operations.md) | Prim as pure infrastructure — metadata in, metadata out | 2026-04-08 | Accepted |
