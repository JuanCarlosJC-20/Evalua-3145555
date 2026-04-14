# Plan de Trabajo Inicial

**Objetivo:** Definir los primeros pasos concretos de ejecución.

## Primera HU Priorizada

### Recomendación: HU-003 o HU-004

**HU-003: Contenerizar PostgreSQL**
- Crear `docker-compose.yml` con servicio PostgreSQL
- Estructurar volúmenes para persistencia
- Documentar cómo conectarse

**HU-004: Contenerizar Liquibase**
- Integrar servicio Liquibase en docker-compose
- Enlazar con la BD PostgreSQL
- Definir punto de entrada para migraciones

## Ejecución en 5 Horas
1. Docker de PostgreSQL funcional
2. Liquibase esqueleto (master + 1 changelog inicial)
3. Documento con plan de migración del SQL a changelogs por dominio

## Siguientes Pasos (Trabajo Desescolarizado)
- Migrar tabla por tabla a Liquibase
- Implementar roles y permisos
- Crear y validar datos de prueba
- Documentar seguimiento completo

