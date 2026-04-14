# Backlog Técnico

**Objetivo:** Registrar historias de usuario y tareas técnicas para la implementación del modelo PostgreSQL
 con buenas prácticas (Docker, Liquibase y organización por dominios).

---

## Historias de Usuario (HU)

| ID     | Descripción                                                              | Prioridad | Dependencias | Estado |
| ------ | ------------------------------------------------------------------------ | --------- | ------------ | ------ |
| HU-001 | Identificar y documentar dominios funcionales del modelo de datos        | Alta      | -            | TODO   |
| HU-002 | Organizar estructura del repositorio y definir ramas (develop, qa, main) | Alta      | -            | TODO   |
| HU-003 | Contenerizar PostgreSQL usando Docker                                    | Alta      | -            | TODO   |
| HU-004 | Contenerizar Liquibase e integrarlo con PostgreSQL                       | Alta      | HU-003       | TODO   |
| HU-005 | Separar el DDL en changelogs organizados por dominio                     | Media     | HU-004       | TODO   |
| HU-006 | Diseñar e implementar roles y permisos en la base de datos               | Media     | HU-003       | TODO   |
| HU-007 | Construir plan de datos de prueba con orden de carga                     | Media     | HU-005       | TODO   |
| HU-008 | Documentar decisiones técnicas y arquitectura del proyecto               | Media     | -            | TODO   |
| HU-009 | Implementar versionado de base de datos con Liquibase                    | Alta      | HU-004       | TODO   |
| HU-010 | Crear scripts de inicialización de base de datos (init.sql)              | Media     | HU-003       | TODO   |
| HU-011 | Validar integridad referencial del modelo (FK, CHECK, UNIQUE)            | Alta      | HU-005       | TODO   |
| HU-012 | Crear índices para optimización de consultas                             | Media     | HU-005       | TODO   |

---

## Tareas Técnicas

### 🔧 Infraestructura

* Configurar Docker Compose con PostgreSQL
* Definir variables de entorno (.env)
* Exponer puertos y volúmenes persistentes
* Configurar red entre contenedores

---

### 🗄️ Base de Datos

* Crear base de datos inicial
* Ejecutar script `modelo_postgresql.sql`
* Validar creación de tablas y constraints
* Verificar claves foráneas

---

### 🔄 Liquibase

* Crear estructura de carpetas:

  * `changelogs/`
  * `changelogs/domains/`
* Crear archivo `master.xml`
* Crear changelogs por dominio:

  * geografia.xml
  * identidad.xml
  * seguridad.xml
  * etc.
* Configurar conexión a la BD
* Ejecutar migraciones

---

### 🧩 Modelado por dominios

* Agrupar tablas por dominio
* Validar dependencias entre dominios
* Separar scripts por orden de ejecución

---

### 🔐 Seguridad

* Crear roles:

  * admin
  * developer
  * readonly
* Asignar permisos por esquema
* Probar accesos

---

### 🧪 Datos de prueba

* Definir orden de inserción (según FK)
* Crear scripts de inserción por dominio
* Validar consistencia de datos

---

### 📊 Optimización

* Crear índices en claves foráneas
* Analizar consultas frecuentes
* Validar rendimiento básico

---

### 📄 Documentación

* Crear `analisis_dominios.md`
* Documentar decisiones técnicas
* Documentar estructura del proyecto
* Incluir instrucciones de ejecución

---

## Estados posibles

* TODO → Pendiente
* IN PROGRESS → En desarrollo
* DONE → Completado

---

## Notas

* El orden de ejecución de dominios es crítico por dependencias FK.
* Liquibase será el mecanismo principal de control de cambios.
* Docker asegura portabilidad del entorno.

---
