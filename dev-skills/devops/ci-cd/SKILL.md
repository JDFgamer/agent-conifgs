# DevOps CI/CD

## Descripción
Define pipelines de GitHub Actions para calidad continua en backend y frontend.

Regla de operación del agente: actuar de forma autónoma en lo cubierto por este skill. Pausar y consultar cuando haya ambigüedad o decisiones de diseño con múltiples alternativas válidas.

## Stack / Tecnologías
- GitHub Actions
- Go + Node.js
- Cache de dependencias

## Estructura recomendada
Pipeline base: `lint -> test -> build` en push y PR, jobs separados backend/frontend.

```text
.github/workflows/ci.yml
```


## Buenas prácticas
- Separar jobs por stack para feedback rápido.
- Cachear módulos de Go y npm.
- ✅ Correcto:
```yaml
- uses: actions/setup-go@v5
  with:
    go-version: '1.24.x'
    cache: true
```
- ❌ Incorrecto: pipeline único gigante con pasos seriales innecesarios.
- No habilitar deploy automático sin aprobación explícita.


## Patrones comunes
- Agregar matriz de versiones (Go/Node) cuando el soporte sea multi-versión.
- Gatear merge con checks requeridos en branch protection.

## Qué evitar
- Mezclar credenciales de despliegue en CI de pruebas.
- Ejecutar despliegue en cada push a cualquier rama.
- Omitir tests por “rapidez” en PRs.

## Cuándo consultar al usuario
- Si se debe agregar un step de deploy a un entorno concreto.
- Si hay que elegir estrategia de promoción (dev -> staging -> prod).
- Si compliance exige aprobaciones manuales adicionales.

## Referencias
- https://docs.github.com/actions
- https://docs.github.com/actions/using-workflows/caching-dependencies-to-speed-up-workflows
