# DevOps Commits (Conventional Commits)

## Descripción
Define formato de commits para historial legible y automatizable.

Regla de operación del agente: actuar de forma autónoma en lo cubierto por este skill. Pausar y consultar cuando haya ambigüedad o decisiones de diseño con múltiples alternativas válidas.

## Stack / Tecnologías
- Conventional Commits
- Git estándar

## Estructura recomendada
Formato obligatorio: `tipo(scope): descripción en minúsculas`.

Tipos válidos: `feat`, `fix`, `chore`, `docs`, `refactor`, `test`, `style`, `ci`.


## Buenas prácticas
- Un commit = un cambio lógico.
- Mensaje corto, claro y accionable.
- ✅ Correcto:
```text
feat(frontend): agrega filtro por categoría en catálogo
fix(backend): corrige validación de email en registro
ci(devops): separa jobs de lint y test
```
- ❌ Incorrecto:
```text
update stuff
WIP
feat: cambios varios
```


## Patrones comunes
- Para cambios grandes, dividir por etapas coherentes: estructura, implementación, tests.
- Mantener scopes alineados con carpetas (`backend`, `frontend`, `devops`, `shared`).

## Qué evitar
- Commits con múltiples objetivos no relacionados.
- Descripciones ambiguas (“arreglos”, “mejoras”).
- Mezclar refactor con feature sin separación.

## Cuándo consultar al usuario
- Nunca. Este skill es 100% determinista.

## Referencias
- https://www.conventionalcommits.org/en/v1.0.0/
- https://git-scm.com/docs/git-commit
