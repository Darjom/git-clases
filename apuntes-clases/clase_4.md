# Clase 4 — **Push, Pull & Pull Request**


## 1 · Push vs Pull, la autopista de dos carriles

| Acción | Comando base | ¿Qué hace? |
|--------|--------------|------------|
| **Enviar** tu historial local al remoto | `git push` | Sube commits/punteros. |
| **Recibir** cambios del remoto | `git pull` | Descarga y fusiona (por defecto). |
| **Sólo descargar** | `git fetch` | Actualiza refs sin tocar tu rama local. |

<p align="center">
  <img src="https://static.platzi.com/media/user_upload/Git%20push-88e4f9c0-01af-4253-b351-5e9a036e5a43.jpg" alt="Push & Pull" width="380">
</p>

> **[Analogía]** *Push = “subir archivos a la nube” / Pull = “bajar la versión más reciente del doc”.*

---

## 2 · Experimento #1 — Empujar desde una rama distinta

```bash
# Estoy en rama `login`
git push origin main
```

**Resultado:** `Everything up‑to‑date`  
El comando intentó subir **main**, no mi rama actual. Para enviar mi rama de trabajo uso:

```bash
git push origin login:main
```

> **[Idea]** *Prefiero crear un Pull Request desde `login` a `main` en lugar de forzar este push directo.*

---

## 3 · Experimento #2 — El misterio del `-u` (o `--set-upstream`)

```bash
git push -u origin login
```

- **Primera vez** que subo la rama `login`.  
- `-u` guarda la relación _tracking_ → futuros `git push` / `git pull` sin parámetros.

> **[Nota mental]** *Una rama nueva sin upstream mostrará “has no upstream branch” cuando intentes `git push` simple.*

---

## 4 · Experimento #3 — El peligro de `git push -f`

`git push -f origin main`

| Pros | Contras (⚠️) |
|------|--------------|
| Re‑escribe historial remoto (p. ej. después de un `rebase`). | Puede **pisar** commits ajenos; provoca conflictos para todos. |

<p align="center">
  <img src="https://preview.redd.it/git-push-force-v0-ky1rwu4yql5a1.jpg?auto=webp" alt="Push force meme" width="250">
</p>

> *Nunca uses `-f` en ramas compartidas sin avisar y consensuar.*

---

## 5 · Experimento #4 — Borrar ramas remotas

```bash
git push origin --delete feature/old‑banner
git push -d origin feature/old‑banner
```

**Recomendaciones antes de borrar:**

1. Confirma que la rama está *mergeada* o ha cumplido su propósito.  
2. Comunica al equipo; quizá alguien la necesite.  

---

## 6 · Traer cambios: `git pull` con estilo

### Comando básico

```bash
git pull              # fetch + merge
```

### Especificar remoto y rama

```bash
git pull origin develop
```

### Sincronizar todo el repositorio

```bash
git pull --all        # todas las ramas remotas -> locales
```

>*Para un historial lineal usa `git pull --rebase` (o configura `pull.rebase=true`).*

---

## 7 · Pull Request (PR) — De la rama a la revisión

1. **Sube tu rama**: `git push -u origin feature/footer`.  
2. GitHub sugiere **“Compare & pull request”**.  
3. Rellena **título** y **descripción** clara.  
4. CI/CD corre & revisores comentan → *Merge*.

### Guía rápida para un buen PR

| Buen hábito | Por qué |
|-------------|---------|
| Commits pequeños y significativos | Facilitan la revisión y revertir si algo falla. |
| Rama actualizada con `main` | Evita conflictos sorpresa. |
| Descripción detallada + “Fixes #ID” | Contexto y vinculación a issues. |
| Screenshots/GIFs si hay UI | El revisor entiende tu cambio al instante. |

### Revisar un PR

* Lee el contexto, no solo el diff.  
* Busca coherencia con las guías de estilo.  
* Comenta **qué** y **por qué**, propone mejoras.  
* Agradece lo bien hecho 🙌.

---

## 8 · Laboratorio guiado

```bash
# 1) Clona repo de práctica
git clone git@github.com:usuario/lab-clase4.git
cd lab-clase4

# 2) Nueva rama de feature
git switch -c feature/header
echo "<header>Clase 4</header>" >> index.html
git add index.html
git commit -m "feat: añade header"

# 3) Sube y crea PR
git push -u origin feature/header
# → Completa PR en GitHub, solicita revisión

# 4) Revisa, aprueba y haz merge en la web
# 5) Trae los cambios a local
git pull
```

> **[Reflexión]** *Con cada PR siento que cuento “una mini‑historia” de lo que hice.*

---

## 9 · Resumen de bolsillo 🗒️

| Comando | Pista mental |
|---------|--------------|
| `git push -u` | “Primera exportación + guardar ruta de envío”. |
| `git pull`    | “Dame lo nuevo y mézclalo”. |
| `git fetch`   | “Sólo escucha, no toques todavía”. |
| `git push -f` | *Peligro* — ré‑escribe historia. |
| PR            | Lugar de debate, pruebas y *merge* limpio. |
