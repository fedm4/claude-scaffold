# claude-scaffold

Base para arrancar un proyecto trabajando con Claude Code.

- `main`: agnóstica. Reglas de trabajo, skills, pr-agent, CI.
- `ts`: `main` + stack TypeScript (pnpm, turbo, eslint, prettier, vitest).

## Usarlo

```sh
git remote add scaffold git@github.com:fedm4/claude-scaffold.git
git pull scaffold main   # o ts
```

Para traer mejoras después, el mismo `git pull`.
