# Upstream originals

Unmodified copies of the third-party skills that `before-dev-skill` was built from. Keep them here to read the original text and to run side-by-side comparisons against `before-dev-skill`.

Files are byte-for-byte copies at the pinned commits below. Do not edit them; to refresh, re-download from a newer commit and update the table.

| Folder | Source | Commit | License |
| --- | --- | --- | --- |
| `mattpocock-skills/` | [mattpocock/skills](https://github.com/mattpocock/skills) | [`d653911`](https://github.com/mattpocock/skills/tree/d65391149dbb5fe76478e76fa8d5122b2aa5a70e) | MIT, see `mattpocock-skills/LICENSE` |
| `superpowers/` | [obra/superpowers](https://github.com/obra/superpowers) | [`22f0b46`](https://github.com/obra/superpowers/tree/22f0b463e7a5e579904dd6211a520321035526f7) | MIT, see `superpowers/LICENSE` |

## What is included

| Skill | Role | Notes |
| --- | --- | --- |
| `grill-me` | Entry point | Only calls `grilling`. User-invoked only (`disable-model-invocation`). |
| `grilling` | The interview itself | Asks questions one decision at a time, ordered by dependency, with a recommended answer. |
| `grill-with-docs` | Entry point | Calls `grilling` + `domain-modeling`. User-invoked only. |
| `domain-modeling` | Glossary and ADRs | Records terms and decisions while grilling; includes `GLOSSARY-FORMAT.md` and `ADR-FORMAT.md`. |
| `to-questionnaire` | Async questions | Turns an open decision into a questionnaire for someone else. User-invoked only. |
| `to-spec` | Synthesis | Turns the conversation into a spec for the issue tracker. User-invoked only. |
| `to-tickets` | Slicing | Breaks a spec into tracer-bullet tickets with blocking edges. User-invoked only. |
| `brainstorming` (Superpowers) | Design dialogue | Reads context, asks questions, proposes approaches, writes a design doc. Includes the optional visual companion (`scripts/` starts a local web server). It hands off to `superpowers:writing-plans`, which is **not** included. |

`agents/openai.yaml` files are the upstream Codex metadata and are kept as-is.

## Trying them out

`grill-me` and `grill-with-docs` call other skills by name, so install the whole set they depend on. Link the folders into your skills directory (Claude Code shown; use `~/.codex/skills` for Codex):

```bash
for s in grill-me grilling grill-with-docs domain-modeling to-questionnaire to-spec to-tickets; do ln -s ~/gitlocal/before-dev-skill/upstream/mattpocock-skills/$s ~/.claude/skills/$s; done
```

```bash
ln -s ~/gitlocal/before-dev-skill/upstream/superpowers/brainstorming ~/.claude/skills/brainstorming
```

Caveats:

- `brainstorming` says it **must** be used before any creative work, and `grilling` triggers on any "grill" phrase. Once installed they will compete with `before-dev-skill` on ordinary requests. Install them only for the comparison and remove the links afterwards.
- For a fair comparison, use the same goal, the same project materials, and the same answers for each skill, ideally in separate fresh sessions.

Remove the links:

```bash
for s in grill-me grilling grill-with-docs domain-modeling to-questionnaire to-spec to-tickets brainstorming; do rm ~/.claude/skills/$s; done
```
