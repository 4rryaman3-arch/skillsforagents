# skillsforagents

Codex skills repository for reusable agent workflows.

## Included Skill

### `pam-configuration-feature`

Implements or updates PAM admin configuration features across the existing stack, including:

- Ember configuration UI
- Django endpoints
- validation constraints
- search metadata
- persisted config values
- scheduler wiring
- background execution logic

## Repository Layout

```text
skills/
  pam-configuration-feature/
    SKILL.md
    agents/openai.yaml
    references/config-patterns.md
config.json
```

## Usage

Use the skill when Codex needs to add or modify a PAM configuration item and should follow the existing application patterns instead of introducing a parallel implementation flow.
