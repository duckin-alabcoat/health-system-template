# ADR-008: Prompt versioning standard

**Date:** 2026-04-11
**Status:** Accepted

## Context

Before April 11, agent prompts had no version numbers. This created two problems:

1. **Debugging:** When an agent behaved unexpectedly, there was no way to know which version of the prompt it was running. Cowork's scheduled tasks embed prompt text (or, in the future, a reference to a prompt file), and without version stamps on output, you couldn't tell if a scheduled run used the current prompt or a stale cached version.

2. **Prim's verification function:** Prim's weekly run is supposed to verify that agents are running current prompts. Without version stamps on agent output, Prim had nothing to compare against.

## Decision

All agent prompts carry a version number. The format is `Major.Minor`:

- **Major bump (X.0):** Fundamental redesign of the agent's role, voice, or approach
- **Minor bump (X.Y):** Additions, tweaks, new sections, rule changes, refinements

Version location: second line of each prompt header, as `### Health System Agent Prompt vX.Y`.

Every agent stamps its version on all output:
- Files with YAML frontmatter: `prompt_version: X.Y` field
- All other output: footer `— [Agent Name] vX.Y`

The architect updates `state/agent_manifest.md` on every prompt version bump — same commit, not a separate step. Prim reads `agent_manifest.md` during her Saturday weekly run for version verification, comparing each agent's stamped output against the manifest.

## Consequences

**Positive:**
- Every piece of agent output is traceable to a specific prompt version
- Prim can detect version mismatches automatically (scheduled agent running old prompt after an edit)
- The architect has a clear discipline: bump the version, update the manifest, commit both together

**Negative:**
- Version discipline requires consistency — the architect must remember to bump on every edit. A silent edit without a version bump will confuse Prim's verification and potentially cause subtle behavioral drift that's hard to attribute
- `state/agent_manifest.md` is in `state/` and therefore gitignored — it's the live source of truth in iCloud but not version-controlled. The prompts themselves are in git; the manifest is not. This creates a small gap where the manifest could drift from the committed prompt versions.
