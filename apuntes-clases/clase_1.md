# Clase 1 — Introducción a Git y Control de Versiones

## 1 · ¿Por qué necesitamos un sistema de control de versiones?

| Beneficio                | ¿Por qué me importa como estudiante?                                          |
|--------------------------|-------------------------------------------------------------------------------|
| **Seguimiento de cambios** | Puedo volver al “antes de romper todo” sin pánico.                           |
| **Colaboración eficiente** | Cuando mi compañero empuje un cambio, entiendo quién hizo qué.              |
| **Seguridad e integridad**| El historial es mi “caja negra”; nada se pierde realmente.                   |
| **Flexibilidad**          | Puedo experimentar en ramas sin miedo a dañar la línea principal.            |

> *Crearé dos archivos `.txt`, haré commits separados y después usaré `git log --oneline` para ver el historial.*

---

## 2 · Git en una frase

![Logo de Git](/img/imagen_git.png)

> “Git es un **sistema de control de versiones distribuido**; cada clon contiene **todo** el historial.”

* Repositorio local completo  →  puedo trabajar *offline*.  
* Luego sincronizo con un repositorio remoto (GitHub, GitLab…).

> **[Sugerencia]** *Probaré `git log --graph --oneline` para visualizar mi árbol cuando empiece a ramificar, ademas tambien hay un juego en itch.io que lo refleja mejor.*

---

## 3 · ¿Qué es un repositorio?

![Estantería de versiones](/img/imagen_estanteria.jpeg)

Un **repositorio** es como una estantería con libros (versiones). Cada commit añade un nuevo libro.

> *Es como guardar “save states” en un videojuego. Por lo que no tengas miedo si algo sale mal*

---

## 4 · Otros sistemas de control de versiones

* GitLab, Bitbucket  
* Mercurial, Bazaar

> **[Curiosidad]** *Podría instalar Mercurial en una VM y comparar comandos equivalentes (`hg commit`).*

---

## 5 · Clases de sistemas de control de versiones

<details>
<summary><strong>Locales</strong></summary>

![Local VCS](ruta/a/imagen_local.png)

* Todo vive en **un solo equipo**.  
* Rápido, pero sin copia de seguridad central.
</details>

<details>
<summary><strong>Centralizados</strong></summary>

![Centralizado VCS](ruta/a/imagen_central.png)

* Un servidor maestro; los clientes suben/bajan cambios.  
* Si el servidor muere… adiós historial.
</details>

<details>
<summary><strong>Distribuidos (Git)</strong></summary>

![Distribuido VCS](ruta/a/imagen_distribuido.png)

* Cada desarrollador tiene el historial completo.  
* Ideal para colaborar sin conexión continua.
</details>

---

## 6 · Los tres estados de Git

| Estado       | Acción típica                   |
|--------------|---------------------------------|
| **Modified** | Edité un archivo.               |
| **Staged**   | `git add archivo`               |
| **Committed**| `git commit -m "mensaje"`       |

> **[Concejo]** *Cuando realizes tu primer commit detente a pensar un poco como lo haces y el orden al inicio me confuncia mucho*

---

## 7 · Primer proyecto paso a paso 🚀

```bash
git init mi-primer-repo          # Crea el repositorio
cd mi-primer-repo
echo "Hola Git" > hola.txt
git add hola.txt                 # Staged
git commit -m "feat: primer commit"  # Committed
```

> *Me acuerdo la primera vez que realize esto, me estaba mueriendo de miedo al no saber que tocar ni que comando usar, mas bien habia chatgpt para ayudarme*

---

## 8 · Conceptos clave para llevarme

1. **Commit = punto de guardado** con metadatos (autor, fecha, mensaje).  
2. **HEAD** es el puntero al commit actual; `git log` lo muestra.  
3. Todo comienza **local**, luego se **sincroniza** con un remoto.

---

## 9 · ¿Y ahora qué puedo intentar?

| Idea “nivel 1”                                                     | Idea “nivel 2”                                                                  |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Crear un alias: `git config --global alias.lg "log --graph --oneline"` | Probar un **hook** `pre-commit` que impida commits sin mensaje claro.          |
| Usar `git commit --amend` para corregir el mensaje del primer commit. | Simular un error y usar `git checkout <id>` para inspeccionar el pasado.       |

> *Entender los **porqués** detrás de cada comando me motiva a practicar más que memorizar sintaxis.*

---

*Clase redactada desde la perspectiva de un estudiante que descubre Git y anota sus momentos “¡aha!” junto a próximos experimentos.*