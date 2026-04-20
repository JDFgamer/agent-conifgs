# Shared Error Handling

## Descripción
Establece lineamientos transversales para manejo de errores y logging seguro.

Regla de operación del agente: actuar de forma autónoma en lo cubierto por este skill. Pausar y consultar cuando haya ambigüedad o decisiones de diseño con múltiples alternativas válidas.

## Stack / Tecnologías
- Go errors
- React error boundaries
- Logging estructurado

## Estructura recomendada
```text
backend: errores de dominio -> mapeo HTTP
frontend: boundaries por sección + fallback UX
```


## Buenas prácticas
- En Go, retornar `error`; evitar `panic` en producción.
- En frontend, usar boundaries por módulo crítico.
- ✅ Correcto:
```go
func ParseAmount(raw string) (int64, error) {
	v, err := strconv.ParseInt(raw, 10, 64)
	if err != nil { return 0, fmt.Errorf("parse amount: %w", err) }
	return v, nil
}
```
- ❌ Incorrecto:
```go
if err != nil { panic(err) }
```
- No loguear PII, passwords, tokens o secrets.


## Patrones comunes
- Clasificar errores: validación, negocio, infraestructura, inesperado.
- Adjuntar `request_id`/`trace_id` en logs y respuestas de error internas.

## Qué evitar
- Catch-all global que oculta contexto.
- Mensajes de error técnicos expuestos al usuario final.
- Logging de payloads sensibles completos.

## Cuándo consultar al usuario
- Si el error debe ser visible al usuario o quedar interno.
- Si una falla externa amerita retry automático o degradación funcional.
- Si compliance requiere redacción específica de mensajes.

## Referencias
- https://go.dev/blog/error-handling-and-go
- https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary
