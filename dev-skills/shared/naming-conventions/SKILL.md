# Shared Naming Conventions

## Descripción
Normaliza convenciones de nombres para Go, TypeScript/JS y estructura de carpetas.

Regla de operación del agente: actuar de forma autónoma en lo cubierto por este skill. Pausar y consultar cuando haya ambigüedad o decisiones de diseño con múltiples alternativas válidas.

## Stack / Tecnologías
- Go
- TypeScript/JavaScript
- Convenciones cross-team

## Estructura recomendada
```text
Go files: snake_case
JS/TS folders: kebab-case
components/types: PascalCase
variables/functions: camelCase
```


## Buenas prácticas
- Go: `camelCase` variables, `PascalCase` exportados, archivos `snake_case`.
- TypeScript/JS: `camelCase` para variables/funciones, `PascalCase` para componentes/tipos.
- Carpetas siempre `kebab-case`.
- ✅ Correcto:
```go
type OrderService struct{}

func newOrderID() string { return "ord_123" }
```
```tsx
type ProductCardProps = { title: string }
export function ProductCard({ title }: ProductCardProps) { return <h3>{title}</h3> }
```
- ❌ Incorrecto: abreviaciones crípticas (`prdSvc`, `usrCtlr`) sin convención.


## Patrones comunes
- Abreviar solo convenciones conocidas: `ctx`, `req`, `res`, `err`.
- Nombrar por intención de negocio (`calculateTotal`) y no por mecánica (`doCalc2`).

## Qué evitar
- Mezclar idiomas en nombres técnicos dentro de la misma capa.
- Renombres constantes sin criterio estable.
- Siglas internas no documentadas que degradan legibilidad.

## Cuándo consultar al usuario
- Si un nombre de dominio tiene dos traducciones válidas.
- Si una convención local del equipo entra en conflicto con este skill.
- Si el contexto regulatorio exige nomenclatura específica.

## Referencias
- https://go.dev/doc/effective_go
- https://www.typescriptlang.org/docs/
- https://github.com/airbnb/javascript
