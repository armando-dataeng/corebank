# CoreBank

Sistema bancario simulado en Oracle para portafolio de DBA + Data Engineering.

## Descripcion

CoreBank es un proyecto educativo y de portafolio que implementa un sistema
transaccional bancario simulado en Oracle Database, con un Data Warehouse en
PostgreSQL alimentado por procesos ETL en Python y visualizado en Power BI.

El objetivo es demostrar, en un contexto realista y defendible ante un
entrevistador tecnico, competencias de:

- Diseno y administracion de bases de datos relacionales (Oracle).
- SQL avanzado con enfasis en transacciones, concurrencia y rendimiento.
- Modelado dimensional y construccion de Data Warehouses.
- Ingenieria de datos con Python.
- Documentacion tecnica profesional y control de versiones con Git.

## Stack tecnologico

| Capa | Tecnologia | Rol |
|------|------------|-----|
| OLTP | Oracle Database Free (Docker) | Sistema transaccional |
| DW | PostgreSQL | Data Warehouse analitico |
| ETL | Python 3.x | Extraccion, transformacion, carga |
| Visualizacion | Power BI | Reportes y dashboards |
| Orquestacion | No incluida en MVP | Planeada para ciclo posterior |
| Control de versiones | Git + GitHub | Trazabilidad y portafolio |

## Arquitectura

┌─────────────────────────────────────────────────────────┐
│                       COREBANK                          │
│                                                         │
│   ┌──────────────┐              ┌──────────────────┐   │
│   │              │              │                  │   │
│   │    OLTP      │    Python    │     OLAP/DW      │   │
│   │   Oracle     │ ────────▶    │   PostgreSQL     │   │
│   │              │   ETL        │                  │   │
│   └──────────────┘              └──────────────────┘   │
│         │                                │             │
│         │                                │             │
│         └────────────┬───────────────────┘             │
│                      ▼                                 │
│                 Power BI                               │
│                                                         │
└─────────────────────────────────────────────────────────┘

## Dominio del negocio

CoreBank modela un banco minorista simplificado con las siguientes entidades:

- Sucursales: unidades operativas donde se registran clientes y cuentas.
- Clientes: personas fisicas o juridicas titulares de cuentas.
- Cuentas: productos bancarios (ahorro, corriente) con condiciones
  contractuales congeladas al momento de apertura.
- Transacciones: depositos, retiros, transferencias y comisiones.
- Transferencias: operaciones atomicas entre dos cuentas.
- Usuarios: personal del banco con roles (cajero, supervisor, auditor, admin).
- Auditoria: registro append-only de operaciones de negocio y de base de datos.

El alcance del MVP excluye explicitamente: prestamos, tarjetas de credito,
cajeros automaticos, fraude, scoring crediticio, banca movil y pagos
internacionales.

## Estructura del repositorio

corebank/
  README.md
  LICENSE
  .gitignore
  CHANGELOG.md
  docs/
    01-contexto-y-objetivos.md
    02-reglas-de-negocio.md
    03-modelo-er.md
    04-arquitectura.md
    05-decisiones-de-diseno/        # ADRs
  database/
    01-tablespaces/
    02-users-roles/
    03-tablas/
    04-constraints/
    05-indexes/
    06-triggers/
    07-sequences/
    08-seed-data/
  oracle/
    docker-compose.yml
    README.md
  lab/
    week-XX/
      README.md
      scripts/
      outputs/
      evidencias/

Las carpetas etl/, postgresql/ y bi/ se agregaran cuando el contenido
correspondiente exista. El historial de Git refleja la evolucion real del
proyecto.

## Estado del proyecto

**Version actual:** v0.1.0 (Semana 1)

| Semana | Hito | Estado |
|--------|------|--------|
| 1 | Setup, reglas de negocio, modelo ER inicial, ADR-001 | En curso |
| 2 | ... | Pendiente |
| 3 | ... | Pendiente |
| 4 | ... | Pendiente |
| 5 | ... | Pendiente |
| 6 | ... | Pendiente |

## Como ejecutar el proyecto

> Pendiente. Se completara en la Semana 2 cuando Oracle Docker este
> configurado.

## Documentacion

- [Reglas de negocio](docs/02-reglas-de-negocio.md)
- [Modelo entidad-relacion](docs/03-modelo-er.md)
- [Decisiones de diseno (ADRs)](docs/05-decisiones-de-diseno/)

## Convenciones del proyecto

- **Ramas**: main, feature/*, fix/*, docs/*, lab/*
- **Commits**: Conventional Commits
- **Versionado**: Semantico (v0.x.0 por hito semanal)
- **Documentacion semanal**: lab/week-XX/README.md

Detalles completos en [ADR-001](docs/05-decisiones-de-diseno/ADR-001-estructura-repositorio.md).

## Licencia

MIT License. Ver [LICENSE](LICENSE).

## Autor

**Ing. Armando Marrero**
Republica Dominicana · Data Engineering & Database Administration

---

"Este proyecto utiliza datos sinteticos generados para fines educativos. No
representa ninguna institucion financiera real y no debe usarse para
operaciones financieras reales."
