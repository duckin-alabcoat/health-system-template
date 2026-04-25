# ADR-001: Use iCloud Drive as the system file store

**Date:** 2026-04-06
**Status:** Accepted

## Context

The health system needs a place to write files that is:
- Accessible from multiple devices (Mac Studio, MacBook, iPhone, iPad)
- Durable across sessions — agents write during a session and Aria reads on the next wake, possibly from a different device
- Writable by Cowork scheduled tasks, which run on the Mac Studio
- Not a database (no infrastructure to manage)

The user already uses iCloud Drive as their primary file system. Cowork mounts iCloud folders directly.

## Decision

All system files live in `iCloud Drive > Health Manager`. Cowork mounts this folder directly. Scheduled tasks on the Mac Studio read and write to it. Interactive sessions on any device read and write to it. iCloud handles sync.

## Consequences

**Positive:**
- Zero infrastructure to manage — no database, no server, no credentials beyond Apple ID
- Multi-device access is automatic
- Works with Cowork's existing folder-mount model

**Negative:**
- iCloud sync is eventually consistent — there is no formal SLA on delivery time between devices
- File eviction can occur on devices without "Keep Downloaded" pinned (mitigated April 11–12 by pinning on both machines)
- Git operations inside an iCloud folder can produce stale lock files that iCloud cannot unlink (observed April 16 — worked around via temp-clone commit pattern)
- No transactions — two agents writing to the same file simultaneously would corrupt it. Mitigated by Single Writer Per State (see ADR-004) and scheduling design (Prim at 3am, Soleil at 3:30am buffer)

**Open question:** If sync latency proves problematic (a scheduled agent fires against a stale prompt), revisit git-pull-before-fire as a fallback. Monitoring window through April 25, 2026.
