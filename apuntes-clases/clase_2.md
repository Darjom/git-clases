# Clase 2 — Commits y Exploración del Historial
## 1 · Repaso ultrarrápido de la Clase 1

* `git init` crea el repositorio.  
* `git add` pasa archivos a **staging**.  
* `git commit` guarda el snapshot localmente.

>  *Hoy vamos a profundizar justo en la línea intermedia: del **add** al **commit** y lo que ocurre después.*

---

## 2 · Anatomía de un commit

| Campo            | Ejemplo                             | ¿Por qué importa?                   |
|------------------|--------------------------------------|-------------------------------------|
| **Hash** (SHA‑1) | `f3b9c0f…`                           | Identificador único ― nunca cambia. |
| **Autor**        | *Daniel Reque `<danijo@gmail.com>`* | Rastro de quién hizo qué.           |
| **Fecha**        | `2025‑05‑09 22:15 −04`               | Línea de tiempo precisa.            |
| **Mensaje**      | `feat: añade sección commits`        | Historia legible para humanos.      |

![Commit layers](/img/step-1-commit.jpg)

> **[Mini‑reto]** *Ejecutar `git show` sobre el último hash y examinar metadatos + diff.*

---

## 3 · Buenas prácticas para mensajes de commit

1. **Imperativo y breve**: “_Añade soporte para login_”, no “_Añadido…_”.  
2. Usa un **prefijo semántico**  
   | Prefijo | Ejemplo | Propósito |
   |---------|---------|-----------|
   | `feat:` | `feat: registrarse con Google` | Nueva funcionalidad |
   | `fix:`  | `fix: corrige bug de sesión`   | Reparación          |
   | `docs:` | `docs: actualiza README`       | Documentación       |
3. Línea de título ≤ 50 caracteres; separa con línea en blanco y cuerpo opcional.

> *Instalar el hook **commitlint** para forzar este formato.*

---

## 4 · Visualizar el historial

| Comando                            | Qué muestra                                  |
|-----------------------------------|----------------------------------------------|
| `git log`                          | Lista completa con hashes y mensajes.        |
| `git log --oneline --graph`        | Gráfico compacto de ramas.                   |
| `git log --stat`                   | Cambios por archivo.                         |
| `git show <hash>`                  | Detalle de un commit específico.             |

![git log graph](/img/log_graf.jpeg)

> *Crea un alias: `git config --global alias.lg "log --graph --oneline --decorate"`, esto ayuda mucho ya que te permite identificarte.*

---

## 5 · Deshacer y corregir sin miedo

| Acción            | Comando                            | Cuándo usarlo                      |
|-------------------|------------------------------------|------------------------------------|
| Editar último commit (mensaje o archivos) | `git commit --amend` | Aún **no** lo has enviado al remoto. |
| Deshacer cambios staged | `git restore --staged archivo` | Te equivocaste de archivo al `add`. |
| Crear nuevo commit que revierte otro | `git revert <hash>`  | Ya lo habías compartido con otros. |
| Volver a un commit y dejar cambios sin guardar | `git reset --soft <hash>` | Re‑acomodar historial local. |

> *Evita `git reset --hard` hasta entender bien sus consecuencias.*

---

## 6 · Laboratorio paso a paso

```bash
# 1. Inicia repo de prueba
mkdir lab-clase2 && cd lab-clase2 && git init

# 2. Crea dos archivos y comitea
echo "apunte 1" > dia1.txt
git add dia1.txt
git commit -m "feat: agrega apunte día 1"

echo "apunte 2" > dia2.txt
git add .
git commit -m "feat: agrega apunte día 2"

# 3. Visualiza historial
git lg                # alias sugerido
```

>*Ver los hashes me hace sentir que cada cambio es “fotografía” inmutable.*

---

## 7 · Conceptos clave para el bolsillo 🗝️

1. **Commit con propósito** → mensaje claro + cambios coherentes.  
2. **Historial = narrativa** de tu proyecto; cuídalo.  
3. Aprender a **revertir** es tu paracaídas frente a errores.  

---

## 8 · Próximos experimentos

| “Pasito” inmediato                                    | Desafío adicional                             |
|-------------------------------------------------------|-----------------------------------------------|
| Usar `git commit -v` para editar mensaje con diff a la vista. | Configurar **gpg‑sign** en tus commits. |
| Editar el último commit con `--amend` y forzar‑push a una rama propia. | Simular conflicto y resolverlo con VS Code.   |

> *Cada commit es una mini‑historia. Quiero que mis historias sean interesantes y fáciles de entender mañana.*