# Backend Infra

## Descripción
Implementa adaptadores técnicos: repositorios, DB, clientes externos y configuración.

Regla de operación del agente: actuar de forma autónoma en lo cubierto por este skill. Pausar y consultar cuando haya ambigüedad o decisiones de diseño con múltiples alternativas válidas.

## Stack / Tecnologías
- Go 1.23+
- PostgreSQL, Redis
- net/http client con timeouts
- Configuración por variables de entorno

## Estructura recomendada
```text
infra/
  db/
    postgres.go
  repository/
    order_postgres.go
  external/
    payment_client.go
  config/
    env.go
```


## Buenas prácticas
- Configurar conexiones con env vars.
- Definir timeout y retry acotado en clientes externos.
- ✅ Correcto:
```go
client := &http.Client{Timeout: 5 * time.Second}
req, _ := http.NewRequestWithContext(ctx, http.MethodGet, url, nil)
resp, err := client.Do(req)
```
- ❌ Incorrecto:
```go
client := &http.Client{} // sin timeout
```
- Circuit breaker básico: cortar tras N fallos consecutivos y reintentar después de ventana de enfriamiento.


## Patrones comunes
```go
func NewPostgresFromEnv() (*sql.DB, error) {
	dsn := os.Getenv("DB_DSN")
	if dsn == "" { return nil, errors.New("DB_DSN is required") }
	return sql.Open("postgres", dsn)
}
```


## Qué evitar
- Hardcodear credenciales.
- Reintentos infinitos sin backoff.
- Acoplar repositorios a structs de handlers/usecases.

## Cuándo consultar al usuario
- Si no está claro qué base de datos elegir.
- Si un servicio externo tiene más de un SDK/protocolo viable.
- Si se requiere resiliencia avanzada (cola, outbox, circuit breaker robusto).

## Referencias
- https://pkg.go.dev/database/sql
- https://12factor.net/config
- https://go.dev/doc/database/
