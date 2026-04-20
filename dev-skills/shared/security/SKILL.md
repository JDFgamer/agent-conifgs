# Shared Security

## Descripción
Reglas mínimas de seguridad para apps web fullstack.

Regla de operación del agente: actuar de forma autónoma en lo cubierto por este skill. Pausar y consultar cuando haya ambigüedad o decisiones de diseño con múltiples alternativas válidas.

## Stack / Tecnologías
- JWT o sesiones
- OWASP ASVS (base)
- CORS configurado explícitamente

## Estructura recomendada
```text
auth/
input validation/
secret management/
cors policy/
```


## Buenas prácticas
- No exponer secretos en frontend.
- Validar inputs siempre en backend, incluso si frontend valida.
- ✅ Correcto:
```go
func ValidateEmail(email string) error {
	if !strings.Contains(email, "@") { return errors.New("invalid email") }
	return nil
}
```
- ❌ Incorrecto:
```ts
const secret = process.env.DB_PASSWORD // usado en cliente
```
- JWT: expiración corta + refresh token seguro; sesiones: cookie `HttpOnly`, `Secure`, `SameSite`.


## Patrones comunes
- CORS mínimo seguro:
```go
w.Header().Set("Access-Control-Allow-Origin", "https://app.example.com")
w.Header().Set("Vary", "Origin")
```
- Rotación periódica de secretos y gestión en vault/secrets manager.

## Qué evitar
- Wildcard CORS en producción sin justificación.
- Guardar tokens en `localStorage` cuando hay alternativa segura con cookies HttpOnly.
- Confiar en validaciones solo del cliente.

## Cuándo consultar al usuario
- Si hay que diseñar un flujo de autenticación nuevo.
- Si se evalúa JWT vs sesión y no hay requerimiento explícito.
- Si una integración externa exige excepciones de CORS o scopes sensibles.

## Referencias
- https://owasp.org/www-project-top-ten/
- https://developer.mozilla.org/docs/Web/HTTP/CORS
- https://datatracker.ietf.org/doc/html/rfc7519
