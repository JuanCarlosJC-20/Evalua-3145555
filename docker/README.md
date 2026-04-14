# Docker - PostgreSQL

## Cómo levantar PostgreSQL

```bash
docker compose up -d
```

## Verificar estado

```bash
docker compose ps
docker compose logs postgres
```

## Conectarse a PostgreSQL

### Usando psql (línea de comandos)
```bash
psql -h localhost -p 5432 -U postgres -d mibase
```

Contraseña por defecto: `postgres123`

### Usando DBeaver u otra herramienta
- Host: `localhost`
- Port: `5432`
- Database: `mibase`
- User: `postgres`
- Password: `postgres123`

## Detener los contenedores

```bash
docker compose down
```

## Persistencia de datos

Los datos se guardan en volumen `postgres_data`. Para borrar todo:

```bash
docker compose down -v
```

## Convenciones

| Variable | Valor por Defecto |
|----------|-------------------|
| Usuario | `postgres` |
| Contraseña | `postgres123` |
| Base de Datos | `mibase` |
| Puerto | `5432` |

> Cambiar en `docker-compose.yml` si es necesario.

