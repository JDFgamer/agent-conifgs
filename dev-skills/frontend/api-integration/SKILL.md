# Frontend API Integration

## Descripción
Define integración segura y tipada entre Next.js y APIs de backend en Go.

Regla de operación del agente: actuar de forma autónoma en lo cubierto por este skill. Pausar y consultar cuando haya ambigüedad o decisiones de diseño con múltiples alternativas válidas.

## Stack / Tecnologías
- Fetch API
- TypeScript
- React Query/SWR opcional

## Estructura recomendada
Centralizar cliente en `lib/api.ts` y modelos en `types/api.ts`.

```text
lib/api.ts
types/api.ts
app/**/page.tsx
```


## Buenas prácticas
- Manejar siempre `loading`, `error`, `success`.
- Tipar requests/responses.
- ✅ Correcto:
```ts
export type Product = { id: string; name: string }

export async function getProducts(): Promise<Product[]> {
  const res = await fetch(`${process.env.NEXT_PUBLIC_API_URL}/products`)
  if (!res.ok) throw new Error(`api error: ${res.status}`)
  return res.json() as Promise<Product[]>
}
```
- ❌ Incorrecto: usar `any` y omitir manejo de estados de error.


## Patrones comunes
- Wrapper de fetch con normalización de errores.
- Retries controlados solo para errores transitorios (timeouts/5xx).

## Qué evitar
- Hardcodear URLs.
- Renderizar datos sin fallback de carga/error.
- Acoplar componentes visuales a detalles del transporte HTTP.

## Cuándo consultar al usuario
- Si la API puede devolver formatos distintos según caso.
- Si el backend no define contrato estable/versionado.
- Si una pantalla necesita estrategia especial de revalidación/cache.

## Referencias
- https://nextjs.org/docs/app/building-your-application/data-fetching
- https://developer.mozilla.org/docs/Web/API/Fetch_API
