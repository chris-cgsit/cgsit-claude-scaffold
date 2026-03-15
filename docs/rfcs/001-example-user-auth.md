# RFC-001: User Authentication

## Status
Draft

## Problem Statement
The application currently has no authentication. All endpoints are publicly accessible. We need user authentication to protect sensitive data and enable user-specific features.

## Proposed Solution
Implement JWT-based authentication with Spring Security. Users register with email/password, receive a JWT on login, and include it in subsequent requests via the `Authorization: Bearer` header.

## Detailed Design

### Data Model
- `users` table: `id`, `email`, `password_hash`, `role`, `created_at`, `updated_at`
- Passwords hashed with bcrypt (strength 12)

### API Endpoints
- `POST /api/v1/auth/register` — Create new account
- `POST /api/v1/auth/login` — Authenticate, returns JWT
- `POST /api/v1/auth/refresh` — Refresh expiring token

### JWT Structure
- Expiry: 15 minutes (access token), 7 days (refresh token)
- Claims: `sub` (user ID), `role`, `iat`, `exp`

## Architecture Impact
- **Affected layers**: API (new auth controller), Service (new auth service), Persistence (new user repository)
- **New packages**: `com.cgsit.[project].security`
- **Database changes**: New `users` table, Flyway migration V002
- **API changes**: All existing endpoints require `Authorization` header, new `/auth/*` endpoints are public

## Security Considerations
- Passwords never stored in plain text — bcrypt only
- JWT secret from environment variable, never hardcoded
- Rate limiting on login endpoint (5 attempts per minute per IP)
- Refresh tokens stored in DB, revocable

## Performance Considerations
- JWT validation is stateless — no DB call per request
- User lookup cached for frequently accessed user data

## Alternatives Considered
- **Session-based auth**: Rejected — not suitable for stateless REST APIs
- **OAuth2 with external provider**: Deferred to a future RFC — adds complexity we don't need yet

## Test Strategy
- **Unit tests**: AuthService (register, login, token generation, validation)
- **Integration tests**: Full auth flow via MockMvc, invalid credentials, expired tokens
- **Edge cases**: Duplicate email registration, SQL injection in email field, malformed JWT

## Open Questions
- Do we need email verification at registration?
- Should we support multiple roles or just user/admin?

## Implementation Checklist
- [ ] Flyway migration for `users` table
- [ ] User entity + UserRepository
- [ ] AuthService (register, login, token handling)
- [ ] JwtTokenProvider utility
- [ ] Spring Security configuration
- [ ] AuthController endpoints
- [ ] Unit tests for AuthService + JwtTokenProvider
- [ ] Integration tests for auth flow
- [ ] Code review completed
