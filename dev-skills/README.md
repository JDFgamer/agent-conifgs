# dev-skills

Biblioteca personal de `SKILL.md` para guiar agentes de IA (por ejemplo Claude Code o Codex) en proyectos fullstack con Go + Next.js.

## ¿Para qué sirve?

Este repositorio define reglas operativas reutilizables para que un agente:

- ejecute tareas de forma consistente por dominio,
- aplique buenas prácticas técnicas,
- y pregunte cuando exista ambigüedad.

Regla transversal de todos los skills:

> Actuar de forma autónoma en lo que esté claramente definido en el skill. Pausar y consultar al usuario cuando algo sea ambiguo, no esté cubierto por el skill, o implique una decisión de diseño con múltiples opciones válidas.

## Cómo usarlo con Claude Code

1. Copiá este repo o añadilo como referencia en tu proyecto.
2. Indicá explícitamente qué skill querés aplicar, por ejemplo:
   - `Usá backend/architecture/SKILL.md para diseñar el servicio`.
   - `Aplicá frontend/design/SKILL.md para la UI mobile-first`.
3. Si necesitás varios dominios, combiná skills por etapa (arquitectura → implementación → testing).

## Índice de skills

### Backend
- `backend/architecture/SKILL.md`: Clean/Hexagonal en Go, capas y regla de dependencias.
- `backend/handler/SKILL.md`: handlers HTTP, validación, contratos JSON y errores.
- `backend/usecase/SKILL.md`: lógica de negocio desacoplada por interfaces.
- `backend/domain/SKILL.md`: entidades, value objects y reglas del dominio.
- `backend/infra/SKILL.md`: repositorios, DB, clientes externos y configuración.
- `backend/testing/SKILL.md`: testing en Go con table-driven tests y testify.

### Frontend
- `frontend/architecture/SKILL.md`: estructura Next.js App Router.
- `frontend/components/SKILL.md`: componentes React tipados y composición.
- `frontend/design/SKILL.md`: mobile-first, accesibilidad y UX.
- `frontend/state/SKILL.md`: decisión entre estado local/global y server state.
- `frontend/api-integration/SKILL.md`: integración API, estados y tipado.
- `frontend/testing/SKILL.md`: unit/component tests con Vitest/Jest + RTL.

### DevOps
- `devops/docker/SKILL.md`: Dockerfiles y docker-compose para stack completo.
- `devops/ci-cd/SKILL.md`: CI en GitHub Actions (lint, test, build).
- `devops/commits/SKILL.md`: Conventional Commits y granularidad de cambios.

### Shared
- `shared/error-handling/SKILL.md`: estrategia de errores y logging seguro.
- `shared/security/SKILL.md`: auth, validación, secretos y CORS.
- `shared/naming-conventions/SKILL.md`: convenciones de naming Go + TS/JS.

## Cómo agregar un nuevo skill

Usá este template mínimo:

```md
# [Nombre del Skill]

## Descripción
Qué cubre este skill y cuándo debe usarse.

## Stack / Tecnologías
Versiones y herramientas relevantes.

## Estructura recomendada
Árbol de carpetas o patrón de código con explicación.

## Buenas prácticas
Lista de reglas concretas con ejemplos de código reales (✅ correcto / ❌ incorrecto).

## Patrones comunes
Ejemplos de implementaciones típicas del dominio.

## Qué evitar
Anti-patterns específicos con explicación de por qué son problemáticos.

## Cuándo consultar al usuario
Lista explícita de situaciones ambiguas donde el agente debe pausar y preguntar.

## Referencias
Links a documentación oficial relevante.
```

## Convención de idioma

- Nombres de carpetas y archivos: **inglés**.
- Contenido de los `SKILL.md`: **español**.
