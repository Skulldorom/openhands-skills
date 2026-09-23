# OpenHands Skills

This repository contains reusable skills for OpenHands.

<p align="center">
  <a href="https://ko-fi.com/skulldorom"><img src="https://ko-fi.com/img/githubbutton_sm.svg" alt="Support me on Ko-fi" /></a>
</p>

## Skills

- `execution-efficiency` — efficient software-engineering workflows and validation.
- `workspace-hygiene` — safe hygiene for permanent, long-lived workspaces.

## Recommended: install from OpenHands

In an OpenHands conversation, run:

```text
/add-skill Skulldorom/openhands-skills/execution-efficiency
```

To install the permanent-workspace skill instead:

```text
/add-skill Skulldorom/openhands-skills/workspace-hygiene
```

OpenHands installs the selected skill into the current workspace's
`.agents/skills/` directory. Start a new conversation if the skill is not
available immediately.

## Manual installation

For execution-efficiency:

```
rm -rf /tmp/openhands-skills && git clone -q https://github.com/Skulldorom/openhands-skills.git /tmp/openhands-skills && mkdir -p /root/.agents/skills && cp -R /tmp/openhands-skills/execution-efficiency /root/.agents/skills/ && rm -rf /tmp/openhands-skills
```

For workspace-hygiene:

```
rm -rf /tmp/openhands-skills && git clone -q https://github.com/Skulldorom/openhands-skills.git /tmp/openhands-skills && mkdir -p /root/.agents/skills && cp -R /tmp/openhands-skills/workspace-hygiene /root/.agents/skills/ && rm -rf /tmp/openhands-skills
```

These install them globally under /root/.agents/skills/
