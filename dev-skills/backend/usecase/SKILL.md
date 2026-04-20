# Backend Usecase

## Descripción
Guía para implementar casos de uso como operaciones de negocio aisladas de transporte y persistencia.

Regla de operación del agente: actuar de forma autónoma en lo cubierto por este skill. Pausar y consultar cuando haya ambigüedad o decisiones de diseño con múltiples alternativas válidas.

## Stack / Tecnologías
- Go 1.23+
- context.Context
- Interfaces de repositorios/servicios

## Estructura recomendada
Un caso de uso por archivo/operación.

```text
usecase/
  create_order.go
  cancel_order.go
  errors.go
```


## Buenas prácticas
- Recibir DTOs de entrada/salida propios del caso de uso.
- Inyectar dependencias por constructor.
- ✅ Correcto:
```go
type PaymentGateway interface { Charge(ctx context.Context, amount int64) error }

type Checkout struct { gateway PaymentGateway }
```
- ❌ Incorrecto:
```go
func (u *Checkout) Execute(w http.ResponseWriter, r *http.Request) {}
```


## Patrones comunes
```go
func (u *Checkout) Execute(ctx context.Context, orderID string) error {
	order, err := u.repo.FindByID(ctx, orderID)
	if err != nil { return err }
	if err := order.ValidateForCheckout(); err != nil { return err }
	return u.gateway.Charge(ctx, order.TotalCents)
}
```


## Qué evitar
- Acceder directo a SQL/ORM desde usecase.
- Importar paquetes HTTP o frameworks web.
- Reutilizar un usecase gigante para múltiples operaciones distintas.

## Cuándo consultar al usuario
- Si la operación cruza múltiples dominios con ownership difuso.
- Si se requieren transacciones distribuidas/compensaciones.
- Si la consistencia esperada (fuerte vs eventual) no está definida.

## Referencias
- https://go.dev/doc/effective_go
- https://martinfowler.com/bliki/AnemicDomainModel.html
