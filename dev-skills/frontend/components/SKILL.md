# Frontend Components

## Descripción
Define cómo construir componentes React reutilizables con Atomic Design simplificado.

Regla de operación del agente: actuar de forma autónoma en lo cubierto por este skill. Pausar y consultar cuando haya ambigüedad o decisiones de diseño con múltiples alternativas válidas.

## Stack / Tecnologías
- React 19+
- TypeScript
- ESLint + Prettier

## Estructura recomendada
```text
components/
  atoms/
  molecules/
  organisms/
```


## Buenas prácticas
- Tipar props siempre.
- Componentes chicos y de responsabilidad única.
- ✅ Correcto:
```tsx
type ButtonProps = { label: string; onClick?: () => void }

export function Button({ label, onClick }: ButtonProps) {
  return <button onClick={onClick}>{label}</button>
}
```
- ❌ Incorrecto:
```tsx
export function Button(props: any) { return <button {...props} /> }
```


## Patrones comunes
- Composición sobre herencia:
```tsx
export function ProductCard({ title, price }: { title: string; price: number }) {
  return <article><h3>{title}</h3><span>${price}</span></article>
}
```


## Qué evitar
- Componentes de 300+ líneas con lógica de negocio embebida.
- Props booleanas ambiguas (`isBig`, `isLarge`, `isPrimary`).
- Side effects dentro del render.

## Cuándo consultar al usuario
- Si un componente empieza a contener lógica de negocio que podría extraerse a hook/usecase frontend.
- Si el nivel atom/molecule/organism no es evidente.
- Si hay conflicto entre reutilización y complejidad de API de props.

## Referencias
- https://react.dev/learn
- https://www.patterns.dev/react/presentational-container-pattern/
