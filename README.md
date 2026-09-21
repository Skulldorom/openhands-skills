# OpenHands Skills

This repository contains reusable skills for OpenHands.

<p align="center">
  <a href="https://ko-fi.com/skulldorom"><img src="https://ko-fi.com/img/githubbutton_sm.svg" alt="Support me on Ko-fi" /></a>
</p>

## Skills

- `execution-efficiency` — efficient software-engineering workflows and validation.
- `workspace-hygene` — safe hygiene for permanent, long-lived workspaces.

## Recommended: install from OpenHands

In an OpenHands conversation, run:

```text
/add-skill Skulldorom/openhands-skills/execution-efficiency
```

To install the permanent-workspace skill instead:

```text
/add-skill Skulldorom/openhands-skills/workspace-hygene
```

OpenHands installs the selected skill into the current workspace's
`.agents/skills/` directory. Start a new conversation if the skill is not
available immediately.

## Manual installation

Copy the skill folder into `.agents/skills/` in the repository where you want
to use it:

```bash
git clone https://github.com/Skulldorom/openhands-skills.git /tmp/openhands-skills
mkdir -p .agents/skills
cp -R /tmp/openhands-skills/execution-efficiency .agents/skills/
```

Replace `execution-efficiency` with `workspace-hygene` to install the other
skill.
