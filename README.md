# before-dev-skill

An agent skill for the phase **before development starts**: it interviews stakeholders (or processes an existing transcript), challenges the premise, drills each rule down to its edge cases, catches contradictions as they appear, and writes everything into traceable documents that developers can plan from.

The goal is simple: after the meeting, two developers reading the same requirement should not build two different things, and nobody should discover a missing role, state, or exception a week before launch.

Works with Claude Code, Codex, and any agent that loads `SKILL.md`-style skills. The skill content itself is written in Chinese; the agent will converse in whatever language the participants use.

## What it does

- **Challenges the premise first.** Before listing features it asks why this is worth doing, what it costs to not do it, how success will be observed, what smaller alternatives exist, what is explicitly out of scope, and who makes the call.
- **Batched rounds, ordered by dependency.** Each round lists every question that can be answered now (usually 3–8), each with what it affects, options, and a recommended answer, so most can be answered with a single letter ("1A 2B 3 not sure"). Questions that depend on an earlier answer wait for the next round. Switch to one-question-at-a-time on request (e.g. live with a decision-maker).
- **Questions aimed at the edges.** Each question restates the rule it heard, then probes the condition most likely to break it with a concrete business example (counter-examples, boundary values, lifecycle, failure and recovery, "5 whys").
- **Live contradiction detection.** Every new answer is checked against recorded decisions, facts, requirements and assumptions; conflicts are raised immediately, not at wrap-up.
- **Never answers on behalf of the business.** Anything the AI infers is logged as an assumption (`H-xx`) until someone confirms it. Recommended answers are offered only as "proposals".
- **Evidence with boundaries.** When there is an existing system, claims are checked against code, API docs, and inventories, and each fact records what it does and does not prove (e.g. a hidden button proves UI visibility, not server-side authorization).
- **Coverage scan.** A 14-dimension checklist (roles & permissions, tenant differences, data lifecycle, failure recovery, integrations, notifications, audit, non-functional, rollout & rollback, acceptance…) plus must-ask questions per requirement type: legacy rewrite / migration, greenfield feature, system integration, process change, reporting.
- **Pre-mortem before wrap-up.** "Three months after launch everyone says this release failed — why?" Answers become risks, missing requirements, or open questions.
- **Explicit Definition of Ready.** Every requirement ends up as *ready for planning*, *blocked*, *deferred*, or *excluded this release*, with the reason.

## Modes

| Mode | What happens |
| --- | --- |
| Pre-meeting prep | Reads existing material, resolves what documents can answer, runs the coverage scan, produces a one-page agenda ordered by decision dependency and impact. |
| Live interview | Hosts the meeting in batched rounds; obeys "skip", "five minutes left", "wrap up", "one at a time". |
| Transcript processing | Extracts attributed statements, decisions, disagreements and gaps from notes or a transcript. |
| Follow-up / change | Resumes from the previous documents, traces impact through stable IDs, keeps superseded decisions. |
| Async questionnaire | Generates prioritized questions for absent stakeholders, each with purpose, options and an optional recommendation. |

## Output

By default, under `docs/requirements/<topic>/` in the target project (or wherever the project already keeps requirements):

- `facts.md` — current behavior, sources, evidence boundaries, glossary (`F-xx`, `T-xx`)
- `requirements.md` — goals and non-goals, decisions, requirements with acceptance scenarios, assumptions, open questions, risks (`D-xx`, `R-xx`, `AC-xx`, `H-xx`, `Q-xx`, `K-xx`)
- `questionnaire.md` — optional, for people not in the room

IDs are stable and never reused, so any requirement can be traced back to the decision, the evidence, and who said it.

## Install

Clone the repo, then symlink it into your agent's skills directory:

```bash
git clone git@github.com:dlyqs/before-dev-skill.git ~/gitlocal/before-dev-skill
```

```bash
ln -s ~/gitlocal/before-dev-skill ~/.claude/skills/before-dev-skill
```

```bash
ln -s ~/gitlocal/before-dev-skill ~/.codex/skills/before-dev-skill
```

## Usage

Prepare a meeting:

> Use before-dev-skill to prepare my requirements meeting with the product owner about X. Read the docs in this project first, resolve anything the documents already answer, and give me a one-page agenda that puts scope-changing questions first.

Host it live:

> Start hosting. Ask each round's independent questions together and wait for our answers; hold dependent questions for the next round. When someone says "same as the old system", challenge it with a concrete exception. Record decisions, objections and open items by speaker.

Wrap up:

> Wrap up. Save facts and requirements to docs/requirements/x/. Give the decision-maker a plain-language read-back, and give developers the list of items ready for planning plus who to ask for each blocker.

Resume later:

> Use before-dev-skill to resume docs/requirements/x/. I have new answers from operations on last time's open questions.

## Files

```
SKILL.md                 workflow, questioning techniques, pacing, hand-off
references/coverage.md   coverage dimensions, per-type must-ask questions, common hidden contradictions
references/records.md    ID scheme, document templates, questionnaire format, Definition of Ready
```

## Credits

Built by combining ideas from:

- Matt Pocock's skills — [`grill-me`, `grilling`, `grill-with-docs`, `domain-modeling`, `to-questionnaire`, `to-spec`, `to-tickets`](https://github.com/mattpocock/skills)
- [Superpowers](https://github.com/obra/superpowers) `brainstorming`
- [Example Mapping](https://cucumber.io/blog/bdd/example-mapping-introduction/) (Cucumber)
- [GitHub Spec Kit](https://github.com/github/spec-kit) `clarify` and `checklist`
- [BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) advanced elicitation (pre-mortem, devil's advocate)
- EARS requirement syntax
