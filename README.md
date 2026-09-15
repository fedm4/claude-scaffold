# claude-scaffold

Base to bootstrap a project working with Claude Code.

- `main`: language-agnostic. Working rules, skills, pr-agent, CI.
- `ts`: `main` + TypeScript stack (pnpm, turbo, eslint, prettier, vitest).

## Usage

```sh
git remote add scaffold git@github.com:fedm4/claude-scaffold.git
git pull scaffold main   # or ts
```

To pull updates later, use the same `git pull`.
