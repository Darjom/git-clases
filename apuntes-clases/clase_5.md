# Clase 5 — Flujos de Trabajo en Git

## 1 · ¿Qué es un **flujo de trabajo**?

Un **workflow** define **cómo** usamos las ramas y **cuándo** las fusionamos.  
Impacta en la velocidad de entrega, la calidad del código y la forma de colaborar.

>*Piensa en los carriles de una autopista: cada modelo decide la cantidad de carriles (ramas) y las reglas para adelantar (merge).*

---

## 2 · GitFlow — el clásico de seis ramas

<p align="center">
  <img src="https://miro.medium.com/v2/resize:fit:800/1*u4dlEq4sqIT6iHL_Usvwnw.png" alt="GitFlow diagram" width="480">
</p>

| Rama          | Nace de | ¿Para qué sirve?                     |
|---------------|---------|--------------------------------------|
| **main**      | —       | Código en producción. Solo *hotfix*. |
| **develop**   | main    | Integración de features; pre‑release.|
| **feature/*** | develop | Nueva funcionalidad.                 |
| **release/*** | develop | Preparar una versión estable.        |
| **hotfix/***  | main    | Arreglo urgente en producción.       |

### Pros y contras

| 👍 Ventajas | 👎 Desventajas |
|-------------|---------------|
| Separación clara entre etapas (dev, QA, prod). | Muchas ramas → complejidad. |
| Historial explícito de lanzamientos. | Lento para proyectos de despliegue continuo. |

>*Úsalo para apps que tienen ciclos de versión “2.1, 2.2, …” y requieren pruebas formales antes de lanzar.*

---

## 3 · GitHub Flow — ligero y continuo

<p align="center">
  <img src="https://user-images.githubusercontent.com/6351798/48032310-63842400-e114-11e8-8db0-06dc0504dcb5.png" alt="GitHub Flow" width="450">
</p>

1. Crea rama **feature/**.  
2. Commits + push frecuentes.  
3. Abre Pull Request contra **main**.  
4. Revisión + CI pasan → *Merge* → despliegue inmediato.

| Pros | Contras |
|------|---------|
| Ciclos cortos; ideal para **CI/CD**. | Muchas ramas pequeñas; gran disciplina de testing. |
| Sencillo de entender. | No registra “versiones” largas por defecto. |

>*Configurar acción de GitHub Actions que despliegue automáticamente al hacer merge en `main`.*

---

## 4 · Trunk‑Based Development — ritmo vertiginoso

<p align="center">
  <img src="https://statusneo.com/wp-content/uploads/2022/08/tbd_workflow.drawio-1-1.png" alt="TBD" width="430">
</p>

- **trunk/main**: Único carril “verdad”.  
- Ramas **short‑lived** (horas), a veces cambios directos al trunk.  
- Necesita pruebas automáticas robustas.

| Pros | Contras |
|------|---------|
| Menos conflictos, integración continua real. | Requiere equipo senior + CI veloz. |
| Facilita *feature flags* para código incompleto. | “Commits rotos” rompen a todos si el pipeline falla. |

>*Lo probaré en un proyecto personal con despliegue a Vercel para sentir la adrenalina.*

---

## 5 · Forking Flow — colaboraciones externas

| Paso | Descripción |
|------|-------------|
| 1. **Fork**       | Copias el repo a tu cuenta GitHub. |
| 2. Clonas tu fork | Trabajas localmente.               |
| 3. Push a tu fork | Rama `feature/...` en tu fork.     |
| 4. PR “fork → upstream” | Mantener core limpio.         |

### Uso típico

- Proyectos **open source** donde colaboradores no tienen acceso directo al repositorio principal.
- Permite revisiones exhaustivas antes de fusionar.

>*Añade el remote original como `upstream` y haz `git fetch upstream` para mantener tu fork al día.*

---

## 6 · Ship / Show / Ask — confianza graduada

| Modo | Para quién | Qué hace |
|------|------------|----------|
| **Ship** | Devs de alta confianza | Push directo a main. |
| **Show** | Devs experimentados | Merge + abre PR para revisión post‑merge. |
| **Ask**  | Devs junior / cambios riesgosos | PR normal, revisión previa. |

>*Interesante para equipos mixtos: regula la “puerta” según experiencia.*

---

## 7 · Elegir el workflow adecuado

| Contexto del proyecto | Recomendación |
|-----------------------|---------------|
| App con releases mensuales, QA formal. | **GitFlow** |
| Startup SaaS, despliegue diario.      | **GitHub Flow** o **TBD** |
| Biblioteca OSS con colaboradores externos. | **Forking Flow** |
| Equipo heterogéneo + CI robusto. | **Ship/Show/Ask** para balancear autonomía y control. |

---

## 8 · Mini‑laboratorio comparativo

| Tarea | GitFlow | GitHub Flow |
|-------|---------|-------------|
| Crear feature “login” | `git switch -c feature/login` | `git switch -c feature/login` |
| Integrar a rama principal | `merge → develop → release → main` | PR directo a `main` tras revisión |
| Despliegue | Al cerrar `release/*`. | Al fusionar PR. |

>*Implementa la misma feature en dos repos —uno con GitFlow, otro con GitHub Flow— y mide tiempo vs. pasos.*

---

## 9 · Conclusiones clave

1. **No existe un único flujo perfecto**; escoge según _velocidad vs. control_.  
2. Cuantas más ramas permanentes tengas, más compleja la fusión.  
3. CI/CD y tests automatizados permiten flujos ligeros y seguros.  
4. Documenta tu decisión para que nuevos miembros la entiendan rápido.

>*Entender los workflows me da “mapas mentales” para navegar cualquier equipo nuevo sin perderme.*