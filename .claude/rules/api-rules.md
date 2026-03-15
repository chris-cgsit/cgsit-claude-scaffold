# API / REST Controller Rules

## Structure
- Controllers are thin — delegate all business logic to service layer
- One controller per aggregate/resource
- Use `@RestController` + `@RequestMapping("/api/v1/[resource]")`

## Request/Response
- Use records for request/response DTOs
- Validate input with `@Valid` + Jakarta Bean Validation annotations
- Never expose entity classes directly — always map to DTOs
- Use `ResponseEntity<T>` for explicit HTTP status control

## HTTP Methods
- `GET` — read, never modify state
- `POST` — create new resources, return `201 Created` with `Location` header
- `PUT` — full update, return `200 OK`
- `PATCH` — partial update
- `DELETE` — return `204 No Content`

## Error Responses
- Use a consistent error response format across all endpoints
- Return `400` for validation errors with field-level details
- Return `404` for missing resources, never `500`
- Return `409` for conflict/duplicate situations

## Documentation
- Document endpoints with OpenAPI/Swagger annotations only if project uses them
- Otherwise, document in the RFC / feature spec
