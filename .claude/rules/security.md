# Security

- Secrets managed via environment variables — never hardcode
- Use parameterized queries for all database access (no string interpolation)
- On security-related code changes, use the `security-reviewer` agent
- Validate all external input at system boundaries
