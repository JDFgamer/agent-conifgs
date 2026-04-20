# Frontend Testing

## Descripción
Guía de testing para frontend con enfoque en comportamiento observable.

Regla de operación del agente: actuar de forma autónoma en lo cubierto por este skill. Pausar y consultar cuando haya ambigüedad o decisiones de diseño con múltiples alternativas válidas.

## Stack / Tecnologías
- Vitest o Jest
- React Testing Library
- Testing de hooks y componentes

## Estructura recomendada
```text
tests/
  unit/
  components/
```


## Buenas prácticas
- Unit tests para funciones puras y hooks.
- Component tests con RTL.
- ✅ Correcto:
```tsx
import { render, screen } from '@testing-library/react'
import { describe, it, expect } from 'vitest'

function Banner({ text }: { text: string }) { return <p>{text}</p> }

describe('Banner', () => {
  it('muestra el texto', () => {
    render(<Banner text="hola" />)
    expect(screen.getByText('hola')).toBeInTheDocument()
  })
})
```
- ❌ Incorrecto: testear métodos internos o estado privado no visible al usuario.


## Patrones comunes
- Usar `msw` para mocks de red en integración de componentes.
- Nombrar tests por comportamiento esperado, no por implementación.

## Qué evitar
- Snapshots masivos sin intención.
- Acoplar tests a clases CSS irrelevantes.
- E2E tempranos para todo el sistema sin priorización de flujos críticos.

## Cuándo consultar al usuario
- Si se debe priorizar qué flujos E2E cubrir primero.
- Si no está claro qué nivel de test aporta más valor (unit vs integration).
- Si el mocking de APIs puede ocultar bugs críticos de contrato.

## Referencias
- https://vitest.dev/
- https://jestjs.io/docs/getting-started
- https://testing-library.com/docs/react-testing-library/intro/
