# Backend Domain

## Descripción
Define entidades, value objects y reglas de negocio puras del dominio.

Regla de operación del agente: actuar de forma autónoma en lo cubierto por este skill. Pausar y consultar cuando haya ambigüedad o decisiones de diseño con múltiples alternativas válidas.

## Stack / Tecnologías
- Go 1.23+
- Errores de dominio con `errors.New`/`fmt.Errorf`
- Sin dependencias de infraestructura

## Estructura recomendada
```text
domain/
  order.go
  money.go
  errors.go
```


## Buenas prácticas
- Modelar comportamiento dentro de entidades.
- Value objects inmutables.
- ✅ Correcto:
```go
type Money struct { cents int64 }

func NewMoney(cents int64) (Money, error) {
	if cents < 0 { return Money{}, ErrInvalidAmount }
	return Money{cents: cents}, nil
}

func (m Money) Cents() int64 { return m.cents }
```
- ❌ Incorrecto:
```go
type Money struct { Cents int64 } // mutable y sin invariantes
```


## Patrones comunes
```go
var ErrOrderAlreadyPaid = errors.New("order already paid")

func (o *Order) MarkPaid() error {
	if o.status == "paid" { return ErrOrderAlreadyPaid }
	o.status = "paid"
	return nil
}
```


## Qué evitar
- Errores HTTP en domain (`ErrBadRequest`, etc.).
- Entidades “anémicas” sin métodos.
- Mezclar tags de DB/JSON en structs de dominio.

## Cuándo consultar al usuario
- Si faltan reglas de negocio en el requerimiento.
- Si una invariante puede interpretarse de más de una forma.
- Si no está claro qué pertenece a entidad vs value object.

## Referencias
- https://go.dev/doc/
- https://dddcommunity.org/
