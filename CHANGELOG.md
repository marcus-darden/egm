# EGM changelog

Covers the six skills (`peanuts`, `elephant`, `goldfish`, `egm-implement`,
`mean-review`, `decide`) and the three subagents in `agents/`. Releases are
git tags; each entry names its tag.

## v1.0.0 (2026-10-12): standalone repo

This repo becomes the only source of the EGM skills. They were previously
kept in the private `marcus-darden/agent-skills` repo and copied into
`eecs498-aase/materials`; the 2026-10-01 entry below was written there.

| Change | Why |
|---|---|
| The five pipeline skills, their walkthroughs and their history move here from `eecs498-aase/materials/egm/`; `decide` and the `generalist`, `reviewer` and `architect` subagents join them. | One source for the suite, owned here and shared with the course, which links to a tag instead of holding a copy. |
| `decide` loses its `tools:` frontmatter key and gets the current shared preamble. | Same reason as the 2026-10-01 entry: `tools:` is not a skill field and breaks claude.ai upload. |
| `reviewer` no longer says "a review that finds nothing is a failed review". | It contradicted the agent's own rule for reporting a clean result, and current models follow such lines literally and invent findings. |
| `egm-implement` hard rule 4 matches Step 6: hand off to `/mean-review`, which the user starts. | The old rule said the work was not complete without mean-review, while Step 6 marked it complete and forbade running mean-review. |
| `elephant`'s interview ends at a playback the user confirms, and the Step 4 debate is "settled" when the user says every objection is answered or accepted. The "20 to 30 minutes", "4 minutes", "2 to 3 days" and "do not rush" lines are gone from the skill. | A model has no clock; elapsed time and turn counts don't measure whether the design is understood. The time estimates stay in the README and walkthroughs, which are for people. |
| Model-facing text is addressed to the model throughout: time estimates and article statistics become conditions the model can observe or figures to tell the user, "you" always means the model, steps only a person can take are phrased as asks to the user, and persuasion aimed at human readers is removed. `goldfish` launches its three reviewers itself; running them by hand is offered only when the runtime cannot start a fresh session. | The skills were partly written in the article's human-facing voice. |
| Prompts the article has a person type to the model (the no-code prompt, the mean-review frame, decide's two openers, peanuts' README prompts, the Step 8 prompt) become instructions. The sycophancy reset is the user's line; every preamble tells the model its job is to challenge the user's thinking and to treat the line as a hard reset. | Recited by the model, those prompts reverse who is speaking ("I am about to describe a feature", "I have a strong intuition this code is of poor quality"). The walkthroughs keep Rensin's wording and explain the change. |
| `bin/package [tag]` builds `dist/<skill>.zip` for claude.ai. | Release zips are reproducible from the tag. |

## 2026-10-01: Agent Skills best-practice pass (in agent-skills, before v1.0.0)

A quality pass, not a redesign. Every hard rule, gate, verbatim prompt,
threshold, ledger field, and status value from the previous version is kept.

Sources, cited by tag below:

- [CC] Claude Code skills docs: https://code.claude.com/docs/en/skills
- [BP] Skill authoring best practices: https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices
- [SPEC] Agent Skills specification: https://agentskills.io/specification
- [AS] Best practices for skill creators: https://agentskills.io/skill-creation/best-practices
- [DESC] Optimizing skill descriptions: https://agentskills.io/skill-creation/optimizing-descriptions
- [SCRIPTS] Using scripts in skills: https://agentskills.io/skill-creation/using-scripts
- [SC] skill-creator and its `quick_validate.py`: https://github.com/anthropics/skills/tree/main/skills/skill-creator
- [EGM] Rensin, "Elephants, Goldfish and the New Golden Age of Software Engineering": https://drensin.medium.com/elephants-goldfish-and-the-new-golden-age-of-software-engineering-c33641a48874

### All five skills

| Change | Why |
|---|---|
| Removed the `tools:` frontmatter key. | Not a skill field. Claude Code ignores unknown keys silently, and claude.ai upload, the Skills API, and `package_skill.py` reject them with a hard error [CC "Using skill frontmatter outside Claude Code"], [SPEC], [SC]. It was not renamed to `allowed-tools`, because `allowed-tools` pre-approves tools without prompting rather than restricting them [CC "Pre-approve tools"], which is the opposite of the old field's intent. |
| Descriptions rewritten in third person ("Runs...", "Validates..."), what it does first, then "Use when..." with the existing trigger phrases, then the EGM phase, then a near-miss pointer to the sibling skill. All 670 to 776 characters. | Third person is required because the description is injected into the system prompt [BP "Writing effective descriptions"]; put the key use case first because listings truncate [CC frontmatter table]; name near-misses so the right skill wins [DESC]; 1,024-character limit [SPEC]. |
| Added `compatibility:` to goldfish, egm-implement, mean-review, and peanuts. | Spec field for environment requirements (subagents, python3, git) [SPEC]. Frontmatter stays inside the six spec fields so the skills still upload to claude.ai [CC]. |
| Phase and step stated in every description and body: elephant Phases 1 and 2, goldfish Phase 3 (Steps 5 to 7), egm-implement Phase 4 Step 8, mean-review Phase 4 Step 9, peanuts the "peanuts and hay" bootstrap. | Consistent terminology across a skill collection [BP "Use consistent terminology"]; numbering checked against [EGM]. |
| A copyable progress checklist at the top of each workflow. | Checklist pattern for multi-step workflows [BP "Use workflows for complex tasks"], [AS "Checklists"]. |
| "Cross-references" replaced by a uniform "Where this skill sits in EGM" section: the pipeline line, then Reads, Writes, Hands off to. | States the handoffs the same way in all five; each skill is self-contained, so each carries its own copy [SPEC directory structure]. |
| Anti-patterns sections added to elephant, mean-review, and peanuts (goldfish and egm-implement already had them), folded from existing prose and tips. | Gotchas are the highest-value content and belong in SKILL.md where they are read before the mistake [AS "Gotchas sections"]. |
| Em dashes removed from all prose; verbatim prompts untouched except one em dash in the Goldfish preamble, now a comma. | House prose rule (aase `AGENTS.md` rule 6). The skills are now published to students. |
| Time-sensitive wording removed ("there is no automated check today", "current model output limits", a pinned model ID in the Hermes config). | Avoid time-sensitive information [BP "Avoid time-sensitive information"]. |
| Shared preamble kept in all five, with reflex 4 now saying "the `/decide` triple ... where that skill is installed". | The duplication is deliberate (not every surface loads `CLAUDE.md`, and a skill cannot link outside its own directory [SPEC "File references"]); `/decide` is not part of this suite. |
| Each body opens with an invocation line, for example `/goldfish [path-to-design-doc]`. | Documents arguments without `argument-hint`, which is Claude Code only and would break claude.ai upload [CC]. |
| `evals/trigger-evals.json` added to each skill: 6 should-trigger and 6 near-miss should-not-trigger queries, in skill-creator's trigger-eval format. Not run yet. | Build evaluations; near-miss negatives test precision [BP "Build evaluations first"], [DESC], [SC]. Excluded from packaging by `package_skill.py`. |

### elephant

- `ELEPHANT.md` header and sections moved to `assets/elephant-ledger-template.md`; the doc skeleton to `assets/design-doc-template.md`; the long per-section guidance, with a weak-versus-better Problem example, to `references/design-doc-sections.md`, read right before drafting each section. Why: progressive disclosure, templates in `assets/` [SPEC], [AS "Templates"]; Step 5 often happens days after the skill loaded, and after compaction only the first 5,000 tokens of a skill are re-attached, so re-reading the reference at that moment is more reliable [CC "Skill content lifecycle"].
- The sycophancy reset line appeared three times; it now lives in reflex 2 and Step 3, and hard rule 3 points to it. Why: concision [BP "Concise is key"].
- Step 6 gains a pre-handoff self-check (sections sized right, every Technical Plan component mapped to a file, no "and similar changes", every alternative has a reason). Why: feedback loop using a checklist as the validator [BP "Implement feedback loops"]. It checks rules the skill already had.
- Step 1 reads a `/peanuts` README hierarchy when one exists, and Step 2 logs decisions under `## Interview decisions`. Why: both are in [EGM] and in the instructor's own `ELEPHANT.md` traces.
- Description adds "Not for one-line fixes or questions about how existing code works." Why: near-miss guidance [DESC].

### goldfish

- Runtime mapping table, cross-runtime notes, and the Hermes profile moved to `references/runtimes.md`; SKILL.md keeps a single default (Claude Code role agents, falling back to `general-purpose` and `Plan`). Why: provide a default, not a menu [AS "Provide defaults, not menus"]; progressive disclosure [BP].
- Preamble and the three reviewer prompts moved verbatim to `assets/goldfish-reviewer-prompts.md`; `GOLDFISH.md` header and round block to `assets/goldfish-ledger-template.md`, with the optional fields the real traces grew (what was under review, verdicts, convergence, contract note, disposition). Why: templates [AS], and the template fixes the field names `check_gate.py` reads.
- Fixed a contradiction: Step 3 said Goldfish C might return "its summary", but C's prompt forbids returning a plan or summary. Step 3 now reports READY, or NOT-READY with the questions.
- Fixed report overwrites: round 2 and later save `goldfish-A-comprehension-r2.md` and so on, as the instructor's traces already do.
- Fixed a dangling "subject to the same rewrites flagged above".
- One home for the human review gate: the `human_review_gate` field in the `GOLDFISH.md` header. Round entries keep a copy; `ELEPHANT.md` keeps a record copy. The old text had elephant saying the other skills read the gate from `ELEPHANT.md`, while goldfish and egm-implement read `GOLDFISH.md`.
- New anti-pattern: a design doc that links into `docs/egm/` lets reviewers read `ELEPHANT.md`. Observed in a real round's contract note. Why: add corrections from real execution [AS "Gotchas sections"].
- Environment facts (today's date, a repo quirk) may be appended identically to all three prompts and recorded in the round entry, never design rationale. Why: matches the instructor's practice in recorded rounds. Round entries still store the prompts verbatim, as before.
- Manual fallback for runtimes without subagents: run each prompt in a new chat. Why: the capability clause [EGM: "Start a brand new, empty AI session"].

### egm-implement

- New `scripts/check_gate.py`: derives the slug, finds `docs/egm/<slug>/`, reads `readiness` from the latest round and the gate from the `GOLDFISH.md` header (YAML frontmatter or the older plain `key: value` block, bold or backticked values), honors `design_doc_path:` in `CLAUDE.md`, and exits 0 GO, 1 BLOCKED, 2 usage, or 3 CHECK-BY-HAND when a value is prose rather than one of the expected words, or when the header gate and the latest round's copy disagree (the first version let the header win with a warning, which could return GO on a ledger whose latest round still said pending; SKILL.md already documented exit 3). Read-only; it never moves a gate. Tested against both real traces in the course workspace. Why: prefer scripts for deterministic, fragile steps [BP "Provide utility scripts"], low freedom on a narrow bridge [BP "degrees of freedom"], meaningful exit codes and no prompts [SCRIPTS].
- `IMPLEMENT.md` skeleton moved to `assets/implement-ledger-template.md`; drift and verification entry formats live there too.
- Opens with the article's Step 8 prompt ("Read this design doc and the files it references. Implement the feature as described. Follow the plan exactly."), matching how elephant and mean-review open with theirs [EGM].
- Verification is now an explicit loop: on failure, log drift, resolve it, fix, re-run, record [BP "Implement feedback loops"].
- Hard rule 2 now says "implement" rather than "touch", so ledger writes and resolved-drift doc edits are plainly allowed, and closing the ledger replaces `status: in-progress` instead of adding a second `status:` line.

### mean-review

- New `scripts/enforced_scans.py` runs the four enforced scans (10-line comment rule, functions over 50 lines, the weak-name regex, silent excepts). Python is parsed with `ast` and `tokenize` and already applies the declarative carve-out; weak names are reported once per name per scope (variables, parameters, attributes, functions, classes, import aliases); an except body "surfaces" the error on a raise, any logger call, or `print`/`warn`/`exit`-style calls. Other languages use line rules and are labeled `[heuristic]`. Step 3 gives the file-list recipe for each scope (branch, working tree, PR, named paths) and says to scan skipped files by hand. The old text asked for a "deterministic" scan using `Grep`, which cannot find uncommented spans. The script passes its own scans. Why: solve, don't punt; deterministic operations belong in scripts [BP "Solve, don't defer", "Provide utility scripts"], [SCRIPTS].
- "Step 2.5" renumbered to Step 3; later steps follow.
- Step 1 reads `IMPLEMENT.md` and the design doc when the diff came from `/egm-implement`, so unlogged drift becomes a finding (this was already in the cross-references).
- Every pass is saved to `docs/egm/<slug>/MEAN-REVIEW.md` (new `assets/mean-review-ledger-template.md`): the scope, counts by tag, the punch list as delivered, and the user's disposition of it, with later passes appended rather than rewritten. Outside EGM the slug comes from the scope. A finding the user rejected with a reason is not raised again without new evidence, and a user who wants no files gets the list in chat with a plain note that nothing was saved. Why: instructor ruling 2026-10-01; the old "Writes: nothing" contradicted reflex 4 (save the artifacts), and a re-run had no record of what the last pass found.

### peanuts

- New `scripts/peanuts_ledger.py` with `plan` (in-scope directories, deepest first), `init` (writes `PEANUTS.md` and records any `--exclude`, never overwrites a ledger), and `next` (review queue, blocked, eligible, gate violations, directories added since init). It never changes a status. A table row that does not parse is reported as a ledger problem rather than skipped, and an in-scope directory missing from the ledger counts as an unfinished child, so neither can open a hole in the gate. Why: the ordering and the gate are deterministic [BP "Provide utility scripts"], plan-validate-execute [BP "Create verifiable intermediate outputs"].
- Resume logic (old Step 4.5) now runs through `next`, which follows it exactly: work the deepest level that still has a `pending` or `needs-human-review` row; if that level has rows awaiting review, review first; otherwise a pending row there is eligible only when every immediate child is `approved` or `rolled-up` (the hard gate).
- The Step 2 handoff message lists the descriptions the model is least sure of, as the old text required, instead of leaving them only in the ledger.
- README shapes for leaf, branch, and root, each with the retirement line, moved to `assets/readme-templates.md`.
- The roll-up prompt restored to the article's exact wording [EGM]; the 5 to 10 minutes of human fixing per leaf added to the warning [EGM].
- Clarified a tension between "never auto-transition to approved" and "closing the task should be the trigger for marking approved": a human's sign-off (their ledger edit, or a review task they closed) is what moves a directory to `approved`.

### Considered and not adopted

- `argument-hint`, `when_to_use`, `disable-model-invocation`, `context: fork`, `hooks`, `paths`: real Claude Code fields [CC], but outside the six spec fields, so packaging and claude.ai upload would fail. A `hooks` entry could someday enforce elephant's no-code rule in Claude Code-only installs.
- `allowed-tools`: pre-approves rather than restricts, and read-only git commands are already allowed without prompting.
- Gerund names (`designing-features`): suggested, not required [BP "Naming conventions"]; renaming would break `/elephant` and the rest.
- `metadata` and `license`: left for the instructor.

### Validation

- `quick_validate.py` from skill-creator: all five pass.
- Frontmatter parsed with PyYAML and Ruby's Psych: all five parse; only `name`, `description`, and `compatibility` present.
- SKILL.md bodies: 117 to 176 lines, roughly 3,000 to 3,900 tokens each, under the 500-line and 5,000-token guidance [SPEC].
- All three scripts: Python 3.9 and 3.12, standard library only, `--help`, no prompts; `enforced_scans.py` reports zero findings on all three.
- A fresh-context reviewer compared the old and new versions file by file, looking only for regressions. Its findings (a gate hole in `next`, level mixing, the gate's two homes, scan accuracy, lost doc lookup and verbatim prompt record, wording contradictions) are fixed above.
