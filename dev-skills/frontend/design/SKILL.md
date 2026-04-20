# Frontend Design (Mobile-first + UX)

## Descripción
Aplica criterios de diseño mobile-first, accesibilidad y performance visual en Next.js.

Regla de operación del agente: actuar de forma autónoma en lo cubierto por este skill. Pausar y consultar cuando haya ambigüedad o decisiones de diseño con múltiples alternativas válidas.

## Stack / Tecnologías
- Tailwind CSS 3+
- next/image
- WCAG 2.1 AA (base)

## Estructura recomendada
Base móvil primero, luego breakpoints ascendentes (`sm`, `md`, `lg`).

```text
design tokens -> tailwind config
components -> clases reutilizables y consistentes
```


## Buenas prácticas
- Definir estilos base para móvil.
- Usar clases utilitarias semánticas (consistentes por sistema).
- ✅ Correcto:
```tsx
export function HeroCTA() {
  return (
    <section className="px-4 py-6 md:px-10 md:py-10">
      <h1 className="text-2xl md:text-4xl font-semibold">Compra rápido</h1>
      <button aria-label="Iniciar compra" className="mt-4 w-full md:w-auto rounded bg-blue-600 px-4 py-3 text-white">
        Comprar ahora
      </button>
    </section>
  )
}
```
- ❌ Incorrecto: estilos inline dispersos sin sistema visual.
- E-commerce UX: CTA principal visible, feedback inmediato (loading/toast), flujo de compra corto.


## Patrones comunes
- Usar `next/image` con dimensiones definidas para evitar CLS.
- Lazy loading para grids extensos y secciones below-the-fold.

## Qué evitar
- Paletas sin contraste suficiente.
- UI desktop-first adaptada “a la fuerza” a móvil.
- Animaciones pesadas en dispositivos móviles modestos.

## Cuándo consultar al usuario
- Si hay que elegir entre dos layouts principales.
- Si existen dos paletas/identidades visuales posibles.
- Si el negocio prioriza conversión vs branding y no hay decisión explícita.

## Referencias
- https://tailwindcss.com/docs
- https://nextjs.org/docs/app/building-your-application/optimizing/images
- https://www.w3.org/WAI/standards-guidelines/wcag/
