# Personal skills

Reusable development workflows. Each top-level skill directory contains its own `SKILL.md`; add future skills as sibling directories.

## Available skills

| Skill | Purpose |
| --- | --- |
| [development-review-loop](development-review-loop/SKILL.md) | GPT-6 Astra Low coordinates and reviews; GPT-5.6 Luna High implements and corrects. Reviews stay within the task's changes and acceptance criteria. |

## Install on macOS or Linux

Clone this repository to any local directory, then link the desired skill into your personal discovery directory. Substitute your clone path below:

```sh
git clone https://github.com/RolandDumit/skills.git
mkdir -p "$HOME/.agents/skills"
ln -s "/absolute/path/to/skills/development-review-loop" "$HOME/.agents/skills/development-review-loop"
```

If the destination already exists, inspect it before changing anything; do not overwrite a different installation. Install only one copy of each skill to avoid duplicate discovery. A copy of the skill folder also works, but must be updated separately.

On Windows, copy `development-review-loop` into `%USERPROFILE%\.agents\skills\` or use a directory link supported by your setup. Recopy it after pulling updates if using a copy.

## Use

Select GPT-6 Astra with Low reasoning in the main conversation, then invoke:

```text
Use $development-review-loop to implement the following task: ...
```

The skill requests GPT-5.6 Luna High for implementation. The host must support those models and native delegation; skill files cannot enable account access or change the running coordinator's settings. Automatic discovery is enabled, but explicit invocation is the most predictable activation method.

A project can reference the installed skill in its operational entry point. Keep architecture, test commands, localization, API, changelog, and delivery requirements in that project's harness. Do not copy private project context into this public repository.

## Update across computers

Run `git pull --ff-only` in each clone. Linked skills use the updated files. If changes are not detected, restart the client or start a new session. Installation and model availability are local to each computer, not synchronized by this repository.

## Verification

The skill defines an instruction-based workflow, not a deterministic scheduler. Review results must be supported by code inspection and appropriate checks. Required checks that cannot run must remain explicitly incomplete.
