---
proyecto: Conocimiento
tipo: indice
estado: en-progreso
area: Fundamentos
tags: [indice, fundamentos]
contenido: completo
---
# Autenticación y autorización

Parte de: [[02-Fundamentos/_Índice Fundamentos|Fundamentos]] · Avance general en [[Temario]].

> [!info] Cómo avanzar
> Cada nota tiene **Autoevaluación** (preguntas con la respuesta plegada). Márcala `visto` cuando la contestes sin abrir las respuestas. Los laboratorios llevan las notas a `dominado`.

## Temas

- [[Autenticación vs autorización]]
- [[Sesiones]]
- [[Tokens de acceso]]
- [[JWT y autenticación con Bearer]]
- [[Firmas de JWT (HS256 vs RS256) y JWKS]]
- [[Rotación de llaves de firma]]
- [[Refresh tokens]]
- [[API keys]]
- [[OAuth 2.0]]
  - [[OAuth 2.0 - roles y scopes]]
  - [[OAuth 2.0 - Client Credentials]]
  - [[OAuth 2.0 - Device Authorization Grant]]
  - [[OAuth 2.0 - flujos obsoletos (Implicit y Password)]]
- [[OAuth 2.0 - Authorization Code con PKCE]]
- [[OpenID Connect]]
- [[Google Sign-In]]
- [[SSO]]
- [[Roles, claims y policies]]
- [[MFA]]
  - [[MFA - TOTP y apps autenticadoras]]
  - [[MFA - passkeys y WebAuthn]]
  - [[MFA - SMS y correo (factores débiles)]]
- [[Hashing de contraseñas]]
- [[Flujo seguro de restablecer contraseña]]
- [[Microsoft Entra ID]]

## Laboratorios

- [[Lab 1 - Decodificar y validar un JWT]]
- [[Lab 2 - Authorization Code con PKCE contra un proveedor de prueba]]

## Progreso

```base
filters:
  and:
    - file.inFolder("02-Fundamentos/05 Autenticación y autorización")
    - file.name != "_Índice Autenticación y autorización"
views:
  - type: table
    name: Temas
    filters:
      and:
        - tipo != "laboratorio"
    order:
      - file.name
      - progreso
      - nivel
  - type: table
    name: Laboratorios
    filters:
      and:
        - tipo == "laboratorio"
    order:
      - file.name
      - progreso
```
