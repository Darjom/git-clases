# Clase 8 — Automatización con Hooks, GitHub Actions y Alias


## 1 · ¿Qué es un *hook*?

Un **hook** es un script que Git ejecuta **automáticamente** cuando ocurre un evento:

| Momento | Hook (lado cliente) | Uso típico |
|---------|---------------------|-----------|
| Antes de *commit* | `pre-commit` | Correr linter, tests, bloquear commits vacíos. |
| Preparar mensaje | `prepare-commit-msg` | Autocompletar plantilla de mensaje. |
| Validar mensaje | `commit-msg` | Enforce Conventional Commits. |
| Después de *commit* | `post-commit` | Notificar en Slack. |
| Antes de *push* | `pre-push` | Ejecutar suite de tests. |

>*Son como “IFTTT” de Git: «Si hago commit, entonces corre ESLint».*

---

## 2 · Añadir un hook local paso a paso

```bash
# 1) En tu proyecto
cd .git/hooks

# 2) Copia de ejemplo
cp pre-commit.sample pre-commit

# 3) Edita `pre-commit` (bash):
#!/bin/sh
npm run lint || {
  echo "❌ Linter errors. Commit aborted."
  exit 1
}

# 4) Hazlo ejecutable
chmod +x pre-commit
```

>*Agrega comprobación de mensaje con `commit-msg` y Regex para prefijos `feat|fix|docs`.*

---

## 3 · Limitaciones de los hooks locales

- **Solo funcionan en tu máquina.**  
- Hay que compartirlos manualmente (`.husky/` o doc).  
- No protegen el branch principal.

---

## 4 · GitHub Actions — hooks en el servidor

<p align="center">
  <img src="https://github.com/SebastianBarreraVargas/Git/blob/main/Imagenes/GithubActionsParte1-1.2.png" alt="GitHub Actions" width="500">
</p>

### Ejemplo: CI básico

`.github/workflows/ci.yml`

```yaml
name: Node CI
on:
  pull_request:
    branches: [ main ]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npm test
```

- Se ejecuta en cada **Pull Request** hacia `main`.  
- Falla el PR si los tests no pasan.

> *Explora el marketplace: hay acciones listas para lint, deploy, enviar tweets, etc.*

---

## 5 · Crear alias para turbo‑productividad

```bash
git config --global alias.co "checkout"
git config --global alias.br "branch"
git config --global alias.st "status -sb"
git config --global alias.fix "commit --amend --no-edit"
```

Ahora:

```bash
git co -b hotfix/logo
git st
```

> **[Idea]** *Agrupa alias en un dotfile `~/.git_aliases` y compártelo con tu equipo.*

---

## 6 · Trucos extra: `stash` y `bisect`

| Comando | Para qué sirve |
|---------|----------------|
| `git stash -m "WIP navbar"` | Guarda cambios sin commitear. |
| `git stash pop` | Recupera último stash y lo borra. |
| `git bisect start / good / bad` | Encuentra commit que introdujo un bug mediante bisección binaria. |

>*Rompe intencionalmente una prueba y usa `bisect` para localizar el commit culpable.*

---

## 7 · Resumen de bolsillo 🗒️

| Herramienta | Recuerda |
|-------------|----------|
| **Hooks locales** | Automatizan pero no viajan con el repo (usa Husky / scripts). |
| **GitHub Actions** | CI/CD en la nube; verifica cada PR. |
| **Alias** | Menos tecleo, más velocidad. |
| **Stash** | “Caja fuerte” temporal de cambios. |
| **Bisect** | Detective privado de bugs.
