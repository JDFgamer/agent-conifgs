# Frontend State

## Descripción
Establece criterios para estado local, global y server state en aplicaciones React/Next.js.

Regla de operación del agente: actuar de forma autónoma en lo cubierto por este skill. Pausar y consultar cuando haya ambigüedad o decisiones de diseño con múltiples alternativas válidas.

## Stack / Tecnologías
- React useState/useReducer
- Zustand o React Context
- React Query o SWR para datos de servidor

## Estructura recomendada
```text
state decision:
UI efímera -> local
compartido entre ramas lejanas -> global
datos remotos -> server state
```


## Buenas prácticas
- `useState` para toggles, modales, formularios locales.
- Estado global solo si múltiples rutas/componentes distantes lo consumen.
- ✅ Correcto:
```tsx
const useCartStore = create<{ count: number; inc: () => void }>((set) => ({
  count: 0,
  inc: () => set((s) => ({ count: s.count + 1 })),
}))
```
- ❌ Incorrecto: guardar respuesta de `GET /products` en store global manual en vez de React Query/SWR.


## Patrones comunes
- Encapsular selectors en hooks (`useCartCount`) para evitar rerenders globales.
- Invalidar cache server-state tras mutaciones exitosas.

## Qué evitar
- Globalizar estado por conveniencia temprana.
- Duplicar estado remoto en múltiples stores.
- Contexts enormes con cientos de propiedades.

## Cuándo consultar al usuario
- Si no está claro si el estado debe ser local o global.
- Si el dato remoto requiere sincronización offline o persistencia compleja.
- Si hay conflicto entre simplicidad y escalabilidad del store.

## Referencias
- https://react.dev/learn/managing-state
- https://zustand.docs.pmnd.rs/
- https://tanstack.com/query/latest
