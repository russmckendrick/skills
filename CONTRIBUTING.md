# Contributing a new skill

1. **Copy the template** into a new directory under `skills/`. The directory name must match the `name:` field in the frontmatter.

   ```sh
   cp -r template skills/your-skill-name
   ```

2. **Edit `skills/your-skill-name/SKILL.md`**:
   - `name`: lowercase letters, numbers and hyphens only, max 64 chars, must equal the folder name.
   - `description`: one sentence covering **what** the skill does *and* **when** the agent should use it. Include trigger keywords — this is the field every agent matches against to decide whether to load your skill. Max 1024 chars.
   - Body: clear instructions, ideally with a `## When to use`, `## Steps` and `## Examples` section. Keep it under ~500 lines; move long reference material into a `references/` subfolder.
   - Optional bundles: `scripts/` for executable code, `references/` for docs the agent loads on demand, `assets/` for templates and data.

3. **Validate locally** before opening a PR:

   ```sh
   npx -y skills-ref validate skills/your-skill-name
   ```

   The same check runs in CI on every push and PR.

4. **Open a PR** describing what the skill does and how you tested it.

## Spec reference

The full Agent Skills specification — required fields, naming rules, progressive disclosure model — lives at <https://agentskills.io/specification>.
