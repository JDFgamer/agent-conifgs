# Frontend Architecture (Next.js App Router)

## Descripción
Organiza aplicaciones Next.js con App Router y separación por responsabilidades.

Regla de operación del agente: actuar de forma autónoma en lo cubierto por este skill. Pausar y consultar cuando haya ambigüedad o decisiones de diseño con múltiples alternativas válidas.

## Stack / Tecnologías
- Next.js 15+
- React 19+
- TypeScript estricto

## Estructura recomendada
```text
src/
  app/
  components/
  hooks/
  lib/
  types/
  store/
```

Usar Server Components por defecto; pasar a Client Component solo cuando haya estado interactivo, efectos o APIs del navegador.


## Buenas prácticas
- Mantener lógica de datos cerca de `app/` (RSC) y UI interactiva en client components.
- ✅ Correcto:
```tsx
// app/products/page.tsx (Server Component)
export default async function ProductsPage() {
  const res = await fetch(`${process.env.API_URL}/products`, { cache: 'no-store' })
  const products: { id: string; name: string }[] = await res.json()
  return <ul>{products.map((p) => <li key={p.id}>{p.name}</li>)}</ul>
}
```
- ❌ Incorrecto: marcar `"use client"` en toda la app sin necesidad.


## Patrones comunes
- Patrón contenedor/presentacional para pantallas complejas.
- Centralizar utilidades HTTP en `lib/api.ts` con tipado compartido.

## Qué evitar
- Duplicar tipos en múltiples carpetas.
- Importar componentes de `app/` dentro de `lib/`.
- Mezclar server-only code con código cliente.

## Cuándo consultar al usuario
- Si hay duda entre RSC y Client Component en un caso específico.
- Si un requisito de SEO/performance choca con interactividad.
- Si una ruta necesita estrategia híbrida sin criterio claro.

## Referencias
- https://nextjs.org/docs/app
- https://react.dev/reference/rsc/server-components
