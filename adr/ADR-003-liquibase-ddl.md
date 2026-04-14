# ADR-003: Liquibase para Versionamiento DDL

**Fecha:** 2026-04-14  
**Estado:** Propuesta

## Contexto
El modelo actual existe como un script SQL único. Para escalar y mantener, se necesita versionamiento de cambios DDL.

## Problema
Un único archivo SQL no permite:
- Rastrear cambios específicos
- Rollback parcial de cambios
- Migraciones incrementales
- Control de versiones efectivo
- CI/CD reproducible

## Decisión
Se adopta **Liquibase** para versionamiento:
- `changelog-master.xml` orquesta los cambios
- Changelogs organizados por dominio funcional
- Cada cambio es un `<changeSet>` atómico
- Se mantiene tabla `DATABASECHANGELOG` para auditoría

## Justificación Técnica
- Liquibase es agnóstico a BD (compatible con PostgreSQL)
- Integración con Docker/CI/CD
- Rollback automático si aplica
- Cada `changeSet` tiene id único y author
- Facilita colaboración en equipo

## Consecuencias / Impacto Esperado
- Nuevas tablas se definen como changelogs XML
- Migraciones se ejecutan con: `liquibase update`
- Historia completa disponible en `DATABASECHANGELOG`
- Requiere aprendizaje YAML/XML de Liquibase
- Rollbacks documentados y controlados

