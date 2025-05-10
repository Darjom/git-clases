# Clase 7 — Deshacer Cambios en Git (sin pánico)

>*Todos rompemos cosas: hoy aprenderé a viajar en el tiempo sin sustos.*  
>*Dominar comandos **destructivos** y **no‑destructivos** para corregir errores y rescatar archivos.*

---

## 1 · Dos filosofías para deshacer

| Categoría | ¿Modifica historial? | ¿Cuándo usar? |
|-----------|----------------------|---------------|
| **No‑destructivos** (`revert`, `checkout`, `stash`) | ✗ | Proyecto compartido; historial público. |
| **Destructivos** (`reset`, `push -f`, `amend`) | ✔ | Solo en local **o** con consenso del equipo. |

>*Si ya “pusheaste”, prefiere métodos no‑destructivos (o coordina un reset global).*

---

## 2 · Comandos no‑destructivos

### 2.1 `git revert`

Crea un **nuevo commit** que revierte otro.

```bash
git revert <hash>
```

- Seguro para producción.  
- Usa `--continue / --abort` cuando haya conflictos.

### 2.2 `git checkout` para rescatar archivos

```bash
git checkout <hash> -- ruta/archivo.txt
```

Trae la versión de un archivo desde un commit pasado sin tocar el HEAD.

### 2.3 Guardar trabajo a medias: `git stash`

```bash
git stash           # guarda todo
git stash pop       # restaura y borra entrada
```

> **[Mini‑reto]** *Guarda cambios con `git stash push -m "WIP navbar"` y enuméralos con `git stash list`.*

---

## 3 · Comandos destructivos (manejar con cuidado)

### 3.1 `git reset`

| Modo | Efecto | Caso de uso |
|------|--------|-------------|
| `--soft`  | Mueve HEAD; **mantiene** cambios en staging. | Re‑agrupar commits antes de push. |
| `--mixed` (por defecto) | Cambios pasan a *Modified*. | Volver a editar antes del commit. |
| `--hard`  | Descartar **todo**; working dir limpio. | Limpiar ensayos locales (¡no publicado!). |

```bash
git reset --soft HEAD~1
git reset --hard <hash>
```

### 3.2 Re‑escribir el último commit

```bash
git commit --amend -m "fix: typo en título"
```

Si ya lo subiste → `git push -f origin rama`.

> **[Alerta]** *Forzar push reescribe historia remota; avisa a tu equipo.*

---

## 4 · Detectar el origen de un bug con `git bisect`

1. `git bisect start`  
2. `git bisect bad` (commit con bug)  
3. `git bisect good <hash‑sin‑bug>`  
4. Compila/prueba → marca `good` / `bad` → Git acota rango.  
5. `git bisect reset` al terminar.

> **[Tip]** *Automatiza con `git bisect run ./script-test.sh`.*

---

## 5 · Tabla de consulta rápida

| Qué quiero hacer | Comando sugerido | Seguro para repo compartido |
|------------------|------------------|-----------------------------|
| Deshacer el último commit **sin** perder cambios | `git reset --soft HEAD~1` | ✅ (antes de push) |
| Publicar corrección del último commit | `git commit --amend` + `push -f` | ⚠️ (avisar) |
| Volver a estado limpio y borrar todo lo no commiteado | `git reset --hard HEAD` | ✅ si local |
| Revertir bug después de desplegar | `git revert <hash>` | ✅ |
| Recuperar archivo borrado hace 3 commits | `git checkout <hash> -- ruta` | ✅ |
| Pausar trabajo a medias | `git stash` | ✅ |

---

## 6 · Laboratorio guiado

```bash
# 1) Repo de prueba
mkdir lab-deshacer && cd lab-deshacer && git init
echo "v1" > app.txt
git add . && git commit -m "feat: versión 1"

# 2) Oops! Commit erróneo
echo "error" >> app.txt
git add . && git commit -m "feat: error terrible"

# 3) Revertir en lugar de reset (repo ya publicado)
git revert HEAD      # crea commit que anula el error

# 4) Ahora prueba reset --soft y --hard en otra rama
git switch -c sandbox
echo "cambio" >> app.txt
git add . && git commit -m "WIP: sandbox"
git reset --soft HEAD~1   # vuelve al estado staged
```

>*Reset “destruye”, revert “cura”: cada uno en su contexto.*

---

## 7 · Conclusiones

1. Antes de borrar, **pregúntate** si ya compartiste el commit.  
2. `revert` agrega historia; `reset` la re‑escribe.  
3. Usa `stash` como clipboard temporal, no como basurero infinito.  
4. **Comunica** cuando necesites forzar push o reset global.
