# F67 skill injection rules

F67 ships no stack knowledge, no best-practice catalogs, and no predefined skills. Everything an agent injects must come from the user's environment or be derived from the working project itself. This file defines only the process.

## Rule 1 — Discover before you inject

For each technical area tagged on the request, search in order and stop at the first match:

1. **Project skills** (`.claude/f67/skills/*.md`) — skills generated or written for this project. Match by filename and first-line description against the area and affected domains.
2. **Installed skills** — the user's installed Claude Code skills and plugins. Match the area against skill names and descriptions. Reference matched skills by name in the spec so downstream agents invoke them.

Never inject skills for areas the request doesn't touch, and never load every available skill.

## Rule 2 — No match means derive from the project, never from priors

When a category has no matching skill, agents must not fall back on generic best-practice opinions. Instead:

- Derive the applicable rules from project evidence: linter/formatter configs, existing code patterns surfaced by discovery, domain memory, decision records, and guidance files (CLAUDE.md, CONTRIBUTING, docs).
- Record the derivation in the execution context so downstream agents follow the project's way, not a canned way.
- If the project itself offers no evidence for the category (truly greenfield), say so explicitly and treat conventions as open questions for the user rather than assuming defaults.

## Rule 3 — Request missing skills

Every unmatched category also becomes a skill request surfaced to the user — during `/f67-init` (from the detected stack) and in the prompt-spec's skills section during workflows. Each request names the category, the project evidence showing it is needed, and the two remedies:

- install a matching skill or plugin from a marketplace, or
- let F67 generate a project skill into `.claude/f67/skills/`.

Record open requests in `config.yaml → skills.requested`; the memory evolver clears entries once a matching skill appears.

## Rule 4 — Generated skills are personalized, evidence-based, and reviewed

When F67 generates a project skill:

- Every rule in it must trace to evidence in this repository (a config, a recurring pattern, a memory entry, a decision). No rule may come from generic training priors.
- Write it in the project's ubiquitous language (glossary, domain memory).
- Mark it `<!-- f67:generated -->` on line 1 and tell the user to review it; the user's edits make it curated.
- Regenerate only on request or when `/f67-sync` finds the evidence has changed; never silently overwrite user edits.
