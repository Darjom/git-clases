# Clase 6 — Buenas Prácticas en Git

> *Puedo moverme por Git, pero… ¿mi historial será legible dentro de seis meses?*  
> *Aprender a escribir commits y ramas que cuenten la historia de mi proyecto sin acertijos.*

---

## 1 · ¿Por qué importan las buenas prácticas?

| Beneficio | Ejemplo concreto |
|-----------|------------------|
| **Legibilidad** | Nuevo teammate entiende el proyecto solo con `git log`. |
| **Resolución de errores** | Volver al commit exacto donde nació un bug es trivial. |
| **Portabilidad** | Adaptarte a repos externos es fácil si dominas las convenciones. |

>*Imagina tu repositorio como un currículum: cada commit es una línea; ¡hazlo entendible!*

---

## 2 · ¿Cada cuánto debo commitear?

*Commits pequeños ≠ commits triviales.*

- **Momento ideal:** cuando completes una unidad lógica de trabajo (p. ej. función, test, documento).  
- Evita commits monolíticos de “+5000 líneas”.  
- **Regla de bolsillo:** *si tardas en describirlo, ¡divídelo!*

---

## 3 · Anatomía de un **buen mensaje de commit**

```
<tipo>: <título en modo imperativo>   (≤ 50 caracteres, sin punto final)

(línea en blanco obligatoria)

Cuerpo opcional con contexto, qué y por qué (72 c/ línea máx.)
Referencias a issues, screenshots o benchmarks.
```

### Tipos frecuentes (Convención **Conventional Commits**)

| Tipo | Uso |
|------|-----|
| **feat** | Nueva funcionalidad de cara al usuario. |
| **fix**  | Corrección de bug. |
| **docs** | Cambios en documentación. |
| **refactor** | Mejora interna sin cambiar comportamiento. |
| **style** | Formato, espacios, linter; sin lógica. |
| **test** | Añade o corrige pruebas. |
| **build/ci** | Cambios en pipeline, dependencias, scripts. |

>*Configura un hook `commit-msg` o **Commitlint** para rechazar mensajes que no sigan este patrón.*

---

## 4 · Ejemplos buenos vs. malos

| ❌ Malo | ✅ Bueno |
|--------|---------|
| `Update` | `docs: corrige enlace roto en README` |
| `arreglos` | `fix: evita crash cuando user = null` |
| `Cambios varios` | *Divide en 2‑3 commits con títulos específicos.* |

---

## 5 · Nombres de rama claros y consistentes

```
<tipo>/<breve-descripcion-kebab>
```

Ejemplos:

| Tipo de rama | Ejemplo |
|--------------|---------|
| **feature/** | `feature/login-oauth` |
| **bugfix/**  | `bugfix/overflow-lista` |
| **docs/**    | `docs/clase6-readme` |

>*Mantén la descripción ≤ 30 caracteres; usa guiones, no espacios.*

---

## 6 · Alias de Git: productividad al instante

```bash
git config --global alias.cm "commit -m"
git config --global alias.lg "log --graph --oneline --decorate"
```

Ahora:

```bash
git cm "feat: agrega alias rápidos"
git lg
```

---

## 7 · Guardar trabajo temporal: `git stash`

```bash
git stash          # guarda y limpia working dir
git switch otra-rama
# …
git switch main
git stash pop      # recupera cambios
```

> **[Uso típico]** *Interrumpido por un hotfix, guardo mi trabajo incompleto sin ensuciar el historial.*

---

## 8 · Resumen de reglas de oro 🏆

1. **Commits atómicos y descriptivos.**  
2. Mensaje en **imperativo**, ≤ 50 c en la cabecera.  
3. **Prefijos semánticos** para tipo de cambio.  
4. Ramas con `tipo/descripcion‑kebab`.  
5. Alias + hooks = flujo rápido y consistente.
