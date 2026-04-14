# ADR-001: Ampliar un Nuevo Dominio Funcional

**Fecha:** 2026-04-14  
**Estado:** Propuesta

## Contexto
Ya existen varios dominios en el modelo. Se requiere definir cómo agregar nuevas funcionalidades sin comprometer la coherencia del sistema.

## Problema
¿Cómo detectar cuándo se necesita un nuevo dominio funcional y cómo integrarlo sin romper la arquitectura existente?

## Decisión
Se adopta un enfoque de **separación clara por dominio funcional**, donde cada dominio:
- Agrupa entidades relacionadas por propósito de negocio
- Mantiene independencia lógica
- Define sus propias tablas, índices y constraints
- Usa changelogs Liquibase independientes

**Dominios sugeridos:**
- [Listar dominios identificados]

## Justificación Técnica
- Escalabilidad: fácil agregar funcionalidad sin cambiar dominios existentes
- Mantenibilidad: cambios localizados por dominio
- Trazabilidad: cada dominio tiene su historia de cambios
- Testing: pruebas independientes por dominio

## Consecuencias / Impacto Esperado
- Nuevas tablas se crean dentro del dominio correspondiente
- Cada dominio tiene su changelog Liquibase
- FK entre dominios se documentan explícitamente
- Facilitará trabajo en equipo futuro (cada equipo = 1 dominio)

