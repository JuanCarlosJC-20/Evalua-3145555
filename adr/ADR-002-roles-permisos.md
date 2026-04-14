# ADR-002: Roles y Permisos Diferenciados

**Fecha:** 2026-04-14  
**Estado:** Propuesta

## Contexto
El sistema necesita control de acceso a datos sensibles y trazabilidad de operaciones por usuario/rol.

## Problema
Sin un modelo explícito de roles y permisos, cualquier aplicación tiene acceso total a la BD, comprometiendo seguridad y auditoría.

## Decisión
Se implementa **RBAC (Role-Based Access Control)** con los siguientes roles:
- `app_admin` - Acceso total, gestión de usuarios/roles
- `app_qa` - Acceso a tablas de prueba y desarrollo
- `app_readonly` - Solo lectura en tablas autorizadas
- `app_writer` - Lectura y escritura en tablas operacionales
- `app_support` - Lectura limitada, solo datos de cliente

## Justificación Técnica
- RBAC es estándar en BD empresariales
- Facilita delegación de permisos por función
- PostgreSQL nativo: `GRANT/REVOKE`
- Permite auditoría de accesos

## Consecuencias / Impacto Esperado
- Necesario usar `search_path` en conexiones
- Aplicación debe usar usuario específico (no superuser)
- Tablas de auditoría con campos `created_by`, `updated_by`
- Vistas puede limitar datos por rol
- CI/CD debe crear usuarios/roles en cada BD

