# Clase 3 — Repositorios Remotos, GitHub y Flujos Básicos de Trabajo

## 1 · ¿Qué es un repositorio remoto?

Un **repositorio remoto** vive en un servidor (GitHub, GitLab, Bitbucket…) y actúa como *punto de sincronización* entre varios clones locales.

<p align="center">
  <img src="https://static.platzi.com/media/user_upload/Git%20push-88e4f9c0-01af-4253-b351-5e9a036e5a43.jpg" alt="Push‑Pull">
</p>

| Ventaja | ¿Por qué me importa? |
|---------|----------------------|
| **Copia de seguridad** | Si mi portátil muere, el código sobrevive en la nube. |
| **Colaboración** | Puedo mezclar mi trabajo con el de otros sin pisarnos. |
| **Integraciones** | Automatizaciones (CI/CD), Issues, GitHub Pages, etc. |

> **[Comparación]** *Piensa en el remoto como la “sucursal central” de una biblioteca: todos podemos llevar y traer libros.*

---

## 2 · Explorando la interfaz de GitHub

| Área | Uso principal |
|------|---------------|
| **Code** | Ver ramas, archivos; editar desde la web. |
| **Pull&nbsp;requests** | Discusión y revisión de cambios antes de fusionar. |
| **Actions** | Despliegue continuo, pruebas automáticas. |
| **Projects** | Tableros Kanban para tareas y road‑map. |
| **Insights** | Estadísticas de actividad y contribuciones. |
| **Settings** | Permisos, ramas protegidas, Webhooks, etc. |

> *Visita “Insights → Community” para comprobar si tu README, licencia e issue templates cumplen las buenas prácticas.*

### Buscar perfiles inspiradores

1. Barra de búsqueda global → selecciona **Users**.  
2. Filtro por lenguaje o ubicación si necesitas algo específico.  

*(Ver capturas en el README original).*

---

## 3 · Creando un repositorio nuevo en GitHub

1. Haz clic en **➕ → New repository**.  
2. Completa **nombre**, **descripción** y selecciona *Public / Private*.  
3. **No marques** “Initialize with README” si ya tienes código local (evitarás conflicto de historial).  
4. Anota la URL **SSH** o **HTTPS** que GitHub muestra al final.

> *Con la GitHub CLI (`gh repo create`) puedes hacerlo desde la terminal.*

---

## 4 · Vincular tu proyecto local con el remoto

### Con HTTPS + token

```bash
git remote add origin https://github.com/usuario/clases-git.git
git push -u origin main   # primer envío
```

> La primera vez Git pedirá usuario y **token personal** (PAT) en vez de contraseña.

### Con SSH (recomendado)

```bash
# Generar clave (una sola vez)
ssh-keygen -t ed25519 -C "tu_correo@example.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Pegar ~/.ssh/id_ed25519.pub en GitHub → Settings → SSH keys
git remote add origin git@github.com:usuario/clases-git.git
git push -u origin main
```

>`ssh -T git@github.com` debe saludar con *“Hi usuario!”*.

---

## 5 · Comandos imprescindibles de sincronización

| Acción | Comando | Notas |
|--------|---------|-------|
| **Enviar commits** | `git push` | Tras `-u`, las próximas veces basta `git push`. |
| **Traer + fusionar** | `git pull` | Equivale a `fetch` + `merge`. |
| **Traer sin fusionar** | `git fetch` | Actualiza refs remotas; ideal para inspeccionar antes de mezclar. |

```bash
git fetch --all
git branch -a       # ver ramas locales y remotas
```

> **[Tip]** *Configura un alias: `git config --global alias.up "pull --rebase --autostash"`.*

---

## 6 · Creación y uso de ramas de trabajo

```bash
git switch -c feature/footer   # crea y cambia
# editar archivos…
git commit -am "feat: add footer"
git push -u origin feature/footer
```

### Estrategias de merge

| Estrategia | Comando | Resulta en… |
|------------|---------|-------------|
| Merge por defecto | `git merge rama` | Commit de merge explícito si hubo divergencia. |
| Rebase + fast‑forward | `git rebase main` | Historial lineal, sin commit extra. |
| `--no-ff` | `git merge --no-ff rama` | Fuerza commit de merge aunque sea FF; útil para PRs. |

> **[Práctica]** *Intenta ambas y compara `git log --graph`.*

---

## 7 · Pull Request (PR) paso a paso

1. Sube tu rama (`git push -u origin feature/footer`).  
2. GitHub sugiere **“Compare & pull request”**.  
3. Añade título + contexto; etiqueta a revisores.  
4. Las comprobaciones de CI deben pasar → **Merge**.

### Cómo escribir un buen PR

* Explica *qué* hace y *por qué*.  
* Commits pequeños y descriptivos.  
* Vincula Issues (`Fixes #12`).  
* Pide revisión a personas concretas.

> *Plantillas de PR en `.github/PULL_REQUEST_TEMPLATE.md` aseguran consistencia.*

---

## 8 · Conflictos comunes y su solución

1. Git marca con `<<<<<<<` / `=======` / `>>>>>>>`.  
2. Elige o combina los cambios.  
3. `git add archivo_resuelto`.  
4. Continúa merge (`git commit`) o rebase (`git rebase --continue`).

<p align="center">
  <img src="https://www.jorgeacortes.com/blog/wp-content/uploads/2019/01/merge-conflict-solve-vs-code.gif" alt="VS Code conflict" width="600">
</p>

>*Activa la extensión “GitLens” para resolver conflictos con un clic.*

---

## 9 · Clonar un repositorio existente

```bash
# HTTPS
git clone https://github.com/otro-usuario/proyecto.git

# SSH
git clone git@github.com:otro-usuario/proyecto.git
```

Si usas **Fork**, clona tu copia y mantén el *remote* original como `upstream` para sincronizarte.

---

## 10 · Laboratorio rápido

```bash
# 1) Crear proyecto local
mkdir lab-remoto && cd lab-remoto && git init
echo "# Lab remoto" > README.md
git add . && git commit -m "docs: init"

# 2) Conectar y subir
git remote add origin git@github.com:usuario/lab-remoto.git
git push -u origin main

# 3) Nueva rama + PR
git switch -c feature/banner
echo "<h1>Banner</h1>" >> index.html
git add index.html && git commit -m "feat: banner inicial"
git push -u origin feature/banner
# → abrir PR en GitHub
```

> *El flujo local‑→ remoto‑→ PR se siente natural tras unos intentos.*

---

## 11 · Resumen para el bolsillo 🗒️

| Concepto | Recordatorio rápido |
|----------|---------------------|
| **origin** | Alias al primer remoto; puedes tener varios. |
| **push -u** | Fija la rama remota (*upstream*) para futuros push/pull. |
| **PR** | Conversación + revisión + merge, todo en uno. |
| **Fetch** | “Dame las novedades, ya veré qué hago con ellas”. |
