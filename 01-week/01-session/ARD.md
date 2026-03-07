# ADR-XXX — Gestión de Ambientes y Control de Cambios por Ramas

## Status
Accepted

## Context

En los repositorios de la organización se requiere establecer una política clara de **gestión de ambientes**, **control de cambios** y **gobernanza de ramas** para evitar modificaciones directas sobre ambientes críticos y garantizar trazabilidad completa entre:

- Historia de Usuario (HU)
- Rama de desarrollo
- Merge Request (MR)
- Ambiente destino

Los ambientes principales del repositorio representan estados controlados del software y **no deben ser alterados directamente** por ningún desarrollador.

Ambientes definidos:

- `develop`
- `qa`
- `main`

Cada uno representa un nivel de estabilidad y control dentro del ciclo de vida del software.

Sin una política formal se presentan riesgos como:

- cambios directos en ramas protegidas
- pérdida de trazabilidad
- despliegues no auditables
- conflictos entre equipos

Por esta razón se define un modelo obligatorio basado en **HU → rama de feature → MR → ambiente padre**.

---

## Decision

Se adopta el siguiente modelo de gobierno de ramas y ambientes.

### 1. Ambientes padre protegidos

Las siguientes ramas se consideran **ambientes padre** y **no pueden ser modificadas directamente**:

- `develop`
- `qa`
- `main`

Reglas:

- No se permiten commits directos
- No se permite push directo
- Solo se permiten cambios mediante **Merge Request**
- Deben configurarse como **Protected Branches**

---

### 2. Cambios obligatoriamente ligados a Historia de Usuario

Todo cambio en el sistema debe originarse desde una **Historia de Usuario (HU)**.

Reglas:

- No se permiten cambios sin HU asociada
- Cada HU genera **una rama de trabajo**
- La rama debe seguir la convención de nombres:
