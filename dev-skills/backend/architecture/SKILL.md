# Backend Architecture (Go Clean/Hexagonal)

## Descripción
Define cómo organizar microservicios Go en capas `domain`, `usecase` e `infra`, separando responsabilidades y dependencias.

Regla de operación del agente: actuar de forma autónoma en lo cubierto por este skill. Pausar y consultar cuando haya ambigüedad o decisiones de diseño con múltiples alternativas válidas.

## Stack / Tecnologías
- Go 1.23+
- net/http o framework liviano (chi/echo)
- Organización por features dentro de capas

## Estructura recomendada
```text
internal/
  order/
    domain/
      entity.go
      repository.go
    usecase/
      create_order.go
    infra/
      postgres_repository.go
      http_handler.go
cmd/api/main.go
```

Regla de dependencias:
- `domain`: sin imports externos de infraestructura.
- `usecase`: puede importar `domain`.
- `infra`: implementa interfaces definidas en `domain` o `usecase`.


## Buenas prácticas
- Definir contratos en capas internas y adaptadores afuera.
- Inyectar interfaces, no concreciones.
- ✅ Correcto:
```go
package usecase

type OrderRepository interface { Save(order any) error }

type CreateOrder struct { repo OrderRepository }

func NewCreateOrder(r OrderRepository) *CreateOrder { return &CreateOrder{repo: r} }
```
- ❌ Incorrecto:
```go
package usecase

import "database/sql"

type CreateOrder struct { db *sql.DB }
```


## Patrones comunes
- Patrón de orquestación desde `main`:
```go
repo := infra.NewPostgresOrderRepository(db)
uc := usecase.NewCreateOrder(repo)
handler := infra.NewOrderHandler(uc)
_ = handler
```


## Qué evitar
- Mezclar lógica de negocio en handler.
- Exponer entidades de DB como modelos de dominio.
- Usar paquetes `utils` genéricos para lógica central (diluye responsabilidades).

## Cuándo consultar al usuario
- Cuando el dominio del negocio no esté claro.
- Cuando una regla pueda vivir en handler o usecase.
- Cuando haya dudas sobre límites de bounded contexts.
- Cuando se evalúe romper la regla de dependencias por “practicidad”.

## Referencias
- https://go.dev/doc/
- https://alistair.cockburn.us/hexagonal-architecture/
- https://8thlight.com/insights/uncle-bob/2012/08/13/the-clean-architecture.html
