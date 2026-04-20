# Backend Handler (HTTP en Go)

## Descripción
Define contratos HTTP, validación y mapeo de errores a status codes para APIs en Go.

Regla de operación del agente: actuar de forma autónoma en lo cubierto por este skill. Pausar y consultar cuando haya ambigüedad o decisiones de diseño con múltiples alternativas válidas.

## Stack / Tecnologías
- Go 1.23+
- net/http, chi o echo
- JSON como formato por defecto

## Estructura recomendada
Mantener handlers como adaptadores finos: parseo request, validación, llamada a usecase, serialización response.

```text
infra/http/
  order_handler.go
  dto.go
  error_mapper.go
```


## Buenas prácticas
- Validar input en borde de entrada.
- Estandarizar respuestas:
- ✅ Correcto:
```go
type APIError struct {
	Code    string `json:"code"`
	Message string `json:"message"`
}

type APIResponse[T any] struct {
	Data  *T       `json:"data,omitempty"`
	Error *APIError `json:"error,omitempty"`
}
```
- ❌ Incorrecto:
```go
w.WriteHeader(500)
w.Write([]byte("algo falló"))
```
- Status sugeridos: `400` validación, `404` no encontrado, `409` conflicto, `422` regla de negocio, `500` interno.


## Patrones comunes
```go
func (h *OrderHandler) Create(w http.ResponseWriter, r *http.Request) {
	var req CreateOrderRequest
	if err := json.NewDecoder(r.Body).Decode(&req); err != nil {
		writeJSON(w, http.StatusBadRequest, APIResponse[any]{Error: &APIError{Code: "bad_request", Message: "json inválido"}})
		return
	}
	out, err := h.uc.Execute(r.Context(), req.CustomerID)
	if err != nil { /* map error */ return }
	writeJSON(w, http.StatusCreated, APIResponse[CreateOrderResponse]{Data: &out})
}
```


## Qué evitar
- Poner reglas de negocio complejas en handlers.
- Devolver errores internos sin sanitizar.
- Variar formato JSON por endpoint sin estándar común.

## Cuándo consultar al usuario
- Si el endpoint requiere lógica de negocio compleja.
- Si existen múltiples formatos de respuesta válidos.
- Si el contrato público debe romper compatibilidad previa.
- Si hay duda entre `409` y `422` por reglas de dominio.

## Referencias
- https://pkg.go.dev/net/http
- https://github.com/go-chi/chi
- https://echo.labstack.com
