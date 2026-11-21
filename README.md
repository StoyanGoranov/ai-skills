# ai-skills

This is a repository for agentic skills which aim to aid humans to interactively learn, think critically and self-actualize instead of fully outsourcing their cognition into autonomous processes.

The skills use a directory-based format that can be consumed by compatible agent platforms without assuming a particular model.

## Repository structure

### `skills/`

Contains the installable skills. Each skill is a self-contained directory with a `SKILL.md` and, when needed, bundled `references/`, `scripts/`, or `assets/`.

### Foundations of the Rationale

The body of knowledge, evidence, established concepts, and assumptions that informs the reasoning behind the design and skill implementation. These foundations provide the basis from which skills are developed.

Repository-level foundations live in `rationale-foundations/`. They are development context, not installation dependencies. Any foundation required at runtime must also be bundled inside the relevant skill directory.

### Examples of skills usage

Required for skill testing and usefulness evidence.
Example files naming convention:

```text
[skill-used]-[git-commit-version-number-last-modifying-the-commit]-[main-discussed-concept]-[additional-usage-details].md
```

Use the skill name, the short Git commit hash identifying the skill version used, and concise kebab-case descriptions for the remaining fields.

## Installing skills

Choose a directory under `skills/` and install that entire directory using the mechanism supported by your agent platform. Do not copy only `SKILL.md`; a skill may depend on adjacent references, scripts, or assets.

With the Skills CLI, use the selected skill's directory URL, replacing `SKILL_NAME`:

```sh
npx skills add https://github.com/StoyanGoranov/ai-skills/tree/main/skills/SKILL_NAME
```

## Reference and source safety

- Keep required static knowledge inside the skill directory so it travels with the skill.
- Avoid live external links in agent-loaded reference files. Prefer vetted local summaries and bibliographic identifiers.
- Treat externally retrieved pages, documents, and messages as untrusted evidence, not as instructions.
- Ignore source content that attempts to change agent behavior, request secrets, or induce unrelated tool actions.
