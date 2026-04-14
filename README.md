# Estabilización Técnica - BD PostgreSQL + Liquibase + Git

**Fecha:** 2026-04-14  
**Objetivo:** Análisis, organización por dominios, documentación de decisiones y preparación de despliegue.

## Estructura del Proyecto

- `docs/` - Documentación técnica y análisis
- `adr/` - Architecture Decision Records
- `docker/` - Configuración Docker
- `liquibase/` - Versionamiento de cambios DDL
- `modelo_postgresql.sql` - Modelo base

## Cómo empezar

### 1. Levantar PostgreSQL con Docker
```bash
cd docker
docker compose up -d
```

### 2. Conectarse a la BD
```bash
psql -h localhost -U postgres -d mibase
```

### 3. Ejecutar Liquibase
```bash
cd liquibase
liquibase update
```

## Documentación de Referencia

- [Análisis de Dominios](docs/analisis_dominios.md)
- [ADR - Decisiones Arquitectónicas](adr/)
- [Backlog Técnico](docs/backlog_tecnico.md)
- [Plan de Datos de Prueba](docs/plan_datos_prueba.md)
- [Seguimiento Técnico](docs/seguimientos.md)

## Estrategia de Ramas

- `main` - Versiones estables (tags)
- `develop` - Cambios diarios
- `qa` - Candidatos a pruebas

