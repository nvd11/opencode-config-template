# Database Services Registry Template

> Record your non-sensitive database endpoints and connection instructions here. Never commit plaintext passwords—use environment variables or secret managers.

## 1. MySQL / PostgreSQL
- **Host**: `<DB_HOST>`
- **Port**: `3306` / `5432`
- **Database**: `<DB_NAME>`
- **Auth**: Loaded via environment variable `DB_PASSWORD`

## 2. Redis
- **Host**: `<REDIS_HOST>`
- **Port**: `6379`
- **Auth**: Loaded via environment variable `REDIS_PASSWORD`
