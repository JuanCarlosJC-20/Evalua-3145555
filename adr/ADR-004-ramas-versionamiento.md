# ADR-004: Estrategia de Ramas, Merges y Versionamiento

**Fecha:** 2026-04-14  
**Estado:** Propuesta

## Contexto
El proyecto necesita flujo de trabajo ordenado para cambios en BD y código, con estabilidad y trazabilidad.

## Problema
Sin estrategia de ramas:
- Cambios sin revisión
- Inestabilidad en main
- Dificultad para identificar cuándo se rompió algo
- No hay margen para QA antes de producción

## Decisión
Se adopta **GitFlow reducido**:

| Rama | Propósito | Quién mergea | Criterio |
|------|-----------|-------------|----------|
| `main` | Versiones estables, tags semánticos | Lead/Release | Merge desde `qa`, sin commits directos |
| `develop` | Integración de cambios del día | Dev Lead | Merge de feature branches tras review |
| `qa` | Candidatos a release, últimas pruebas | QA Lead | Merge desde `develop`, pasa validación |
| `feature/*` | Cambios individuales (ramas votivas) | Developer | Borrar tras merge a `develop` |

**Versionamiento:** Semántico (MAJOR.MINOR.PATCH) con tags en `main`.

## Justificación Técnica
- GitFlow es estándar en equipos distribuidos
- Evita commits directos a main
- Facilita rollback a versión anterior (tag)
- Cada rama tiene propósito claro

## Consecuencias / Impacto Esperado
- Requiere disciplina de merge requests
- Historia clara de cambios por rama
- CI/CD puede validar antes de merge
- Rollback a versión anterior es trivial (`git checkout v1.0.0`)

