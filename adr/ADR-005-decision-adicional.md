# ADR-005: Estrategia de Datos de Prueba (Idempotencia y Dependencias)

**Fecha:** 2026-04-14  
**Estado:** Propuesta

## Contexto
El proyecto requiere datos de prueba reproducibles, que se carguen sin errores y en el orden correcto según dependencias de FK.

## Problema
- Sin orden definido, inserciones fallan por FK violadas
- Sin idempotencia, re-ejecutar se quiebra por duplicados
- Manual seed es propenso a errores

## Decisión
Se define **estrategia de seed idempotente**:

1. **Fase 1: Tablas de Referencia** (sin FK externas)
   - `seed_001_referencia.sql` - geografía, tipos, catálogos

2. **Fase 2: Tablas Base** (FK solo a referencia)
   - `seed_002_identidad.sql` - usuarios, roles

3. **Fase 3: Tablas Operacionales** (FK a bases anteriores)
   - `seed_003_clientes.sql` - clientes, contactos

4. **Fase 4: Transacciones** (FK a operacionales)
   - `seed_004_operaciones.sql` - ordenes, movimientos

**Idempotencia:** Cada script usa `ON CONFLICT DO NOTHING` o verifica existencia antes de insertar.

## Justificación Técnica
- Reproduce bugs locales
- Facilita QA consistente
- CI/CD puede usar el mismo seed
- Rollback es trivial (borrar y re-ejecutar)

## Consecuencias / Impacto Esperado
- Seeds son archivos versionados en Git
- Se ejecutan con Liquibase o script manual
- Datos de prueba siempre en estado conocido
- Facilita debugging y desarrollofuturo

