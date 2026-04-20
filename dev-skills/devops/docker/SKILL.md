# DevOps Docker

## Descripción
Define contenedorización para backend Go y frontend Next.js en entorno local y CI.

Regla de operación del agente: actuar de forma autónoma en lo cubierto por este skill. Pausar y consultar cuando haya ambigüedad o decisiones de diseño con múltiples alternativas válidas.

## Stack / Tecnologías
- Docker 26+
- docker compose v2
- Go + Next.js standalone

## Estructura recomendada
```text
docker/
  backend.Dockerfile
  frontend.Dockerfile
docker-compose.yml
.env.example
```


## Buenas prácticas
- Multi-stage para imágenes pequeñas.
- Nunca hardcodear secretos.
- ✅ Correcto (Go):
```dockerfile
FROM golang:1.24 AS build
WORKDIR /src
COPY . .
RUN CGO_ENABLED=0 go build -o app ./cmd/api

FROM gcr.io/distroless/static-debian12
COPY --from=build /src/app /app
ENTRYPOINT ["/app"]
```
- ✅ Correcto (Next.js standalone):
```dockerfile
FROM node:22-alpine AS build
WORKDIR /app
COPY . .
RUN npm ci && npm run build

FROM node:22-alpine
WORKDIR /app
COPY --from=build /app/.next/standalone ./
COPY --from=build /app/.next/static ./.next/static
CMD ["node", "server.js"]
```


## Patrones comunes
- Compose local con `backend`, `frontend`, `postgres`, `redis` en red interna.
- Definir healthchecks en servicios de estado (DB/redis).

## Qué evitar
- Imagen monolítica con toolchain completa en runtime.
- Variables sensibles dentro del Dockerfile.
- `latest` sin pin de versiones base.

## Cuándo consultar al usuario
- Si hay servicios adicionales no contemplados (broker, observabilidad, etc.).
- Si se requiere estrategia multi-arch o seguridad endurecida.
- Si hay dudas de volumenes y persistencia para desarrollo.

## Referencias
- https://docs.docker.com/build/building/multi-stage/
- https://nextjs.org/docs/app/api-reference/config/next-config-js/output
- https://docs.docker.com/compose/
