# OpenHands Skills

Reusable [Agent Skills](https://agentskills.io/) for OpenHands. Each skill is
kept in its own directory and starts at `SKILL.md`.

## Available skills

| Skill | Purpose |
| --- | --- |
| [`execution-efficiency`](execution-efficiency/SKILL.md) | Efficient software-engineering execution, workspace preparation, cleanup, and validation guidance. |

## Add a skill to a repository

The most portable installation is to copy the skill into the repository where
OpenHands will use it. OpenHands discovers project skills from `.agents/skills/`.

From the target repository, run:

```bash
mkdir -p .agents/skills
git clone <this-repository-url> /tmp/openhands-skills
cp -R /tmp/openhands-skills/execution-efficiency .agents/skills/
```

On PowerShell, the equivalent is:

```powershell
New-Item -ItemType Directory -Force .agents\skills | Out-Null
git clone <this-repository-url> "$env:TEMP\openhands-skills"
Copy-Item -Recurse "$env:TEMP\openhands-skills\execution-efficiency" .agents\skills\
```

Replace `<this-repository-url>` with the URL of this repository. Commit the
copied skill to the target repository so other OpenHands sessions and
contributors receive the same instructions. Start a new OpenHands session if
the current session does not pick up the new skill automatically.

To install another skill, replace `execution-efficiency` in the commands with
that skill's directory name.

`.openhands/skills/` is also used by older OpenHands installations. If the
client you use expects that legacy location, copy the skill there instead.

## Add a skill to this collection

Create one directory per skill:

```text
<skill-name>/
└── SKILL.md
```

Keep `SKILL.md` concise and operational. Add supporting `references/` or
`scripts/` only when they are needed by the skill, and document those files in
the skill itself. Test a new skill in a small repository before submitting it.

Skills contain instructions that an agent may follow with repository access.
Review a skill before installing it, especially when it includes commands that
can modify or delete files.
