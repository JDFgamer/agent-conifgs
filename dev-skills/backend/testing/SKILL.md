# Backend Testing (Go)

## Descripción
Define estrategia de pruebas en Go con foco en table-driven tests y uso de testify.

Regla de operación del agente: actuar de forma autónoma en lo cubierto por este skill. Pausar y consultar cuando haya ambigüedad o decisiones de diseño con múltiples alternativas válidas.

## Stack / Tecnologías
- Go testing package
- github.com/stretchr/testify/assert
- github.com/stretchr/testify/mock

## Estructura recomendada
```text
internal/order/
  usecase/create_order_test.go
  infra/order_repository_integration_test.go
```

Cobertura mínima sugerida:
- Domain: 90%+
- Usecase: 85%+
- Infra: 70%+


## Buenas prácticas
- Usar table-driven tests como patrón obligatorio.
- Mockear interfaces en pruebas unitarias.
- ✅ Correcto:
```go
func TestNewMoney(t *testing.T) {
	cases := []struct{name string; input int64; wantErr bool}{
		{"ok", 100, false},
		{"negative", -1, true},
	}
	for _, tc := range cases {
		t.Run(tc.name, func(t *testing.T) {
			_, err := NewMoney(tc.input)
			if tc.wantErr { assert.Error(t, err) } else { assert.NoError(t, err) }
		})
	}
}
```
- ❌ Incorrecto: un solo test gigante con múltiples asserts no aislados.


## Patrones comunes
```go
type RepoMock struct { mock.Mock }

func (m *RepoMock) Save(ctx context.Context, o Order) error {
	args := m.Called(ctx, o)
	return args.Error(0)
}
```


## Qué evitar
- Conectar DB real en unit tests.
- Testear solo “happy path”.
- Ignorar assertions de errores de negocio.

## Cuándo consultar al usuario
- Si una prueba necesita DB real o servicio externo y no está definido alcance.
- Si hay duda sobre qué flujos priorizar en e2e.
- Si el objetivo de cobertura impacta tiempos de CI de forma significativa.

## Referencias
- https://pkg.go.dev/testing
- https://github.com/stretchr/testify
