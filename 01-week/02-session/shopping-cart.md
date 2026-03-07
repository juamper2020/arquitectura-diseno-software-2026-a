# Modelo de Entidades — Security, Inventory, Billing

Supuesto: modelo base para un sistema ERP simple con módulos de **seguridad**, **inventario** y **facturación**.  
Se incluyen claves primarias, claves foráneas y atributos comunes de auditoría.

---

# 1. Security

## Person

| Atributo | Tipo | Descripción |
|---|---|---|
| id | UUID (PK) | Identificador único de la persona |
| first_name | VARCHAR | Nombre |
| last_name | VARCHAR | Apellido |
| document_type | VARCHAR | Tipo de documento |
| document_number | VARCHAR | Número de documento |
| email | VARCHAR | Correo electrónico |
| phone | VARCHAR | Teléfono |
| status | VARCHAR | Estado del registro |
| created_at | TIMESTAMPTZ | Fecha de creación |
| updated_at | TIMESTAMPTZ | Fecha de actualización |

---

## User

| Atributo | Tipo | Descripción |
|---|---|---|
| id | UUID (PK) | Identificador del usuario |
| person_id | UUID (FK) | Referencia a Person  UNIQUE|
| username | VARCHAR | Nombre de usuario |
| password_hash | VARCHAR | Contraseña en hash |
| status | VARCHAR | Estado del usuario |
| last_login | TIMESTAMPTZ | Último acceso |
| created_at | TIMESTAMPTZ | Fecha de creación |
| updated_at | TIMESTAMPTZ | Fecha de actualización |

---

## Role

| Atributo | Tipo | Descripción |
|---|---|---|
| id | UUID (PK) | Identificador del rol |
| name | VARCHAR | Nombre del rol |
| description | VARCHAR | Descripción |
| status | VARCHAR | Estado |
| created_at | TIMESTAMPTZ | Fecha de creación |
| updated_at | TIMESTAMPTZ | Fecha de actualización |

---

## Permission

| Atributo | Tipo | Descripción |
|---|---|---|
| id | UUID (PK) | Identificador del permiso |
| name | VARCHAR | Nombre del permiso |
| description | VARCHAR | Descripción |
| resource | VARCHAR | Recurso del sistema |
| action | VARCHAR | Acción permitida |
| status | VARCHAR | Estado |
| created_at | TIMESTAMPTZ | Fecha de creación |
| updated_at | TIMESTAMPTZ | Fecha de actualización |

---

# 2. Inventory

## Category

| Atributo | Tipo | Descripción |
|---|---|---|
| id | UUID (PK) | Identificador de la categoría |
| name | VARCHAR | Nombre de la categoría |
| description | VARCHAR | Descripción |
| status | VARCHAR | Estado |
| created_at | TIMESTAMPTZ | Fecha de creación |
| updated_at | TIMESTAMPTZ | Fecha de actualización |

---

## Product

| Atributo | Tipo | Descripción |
|---|---|---|
| id | UUID (PK) | Identificador del producto |
| name | VARCHAR | Nombre del producto |
| description | TEXT | Descripción |
| sku | VARCHAR | Código del producto |
| price | NUMERIC | Precio |
| category_id | UUID (FK) | Categoría del producto |
| status | VARCHAR | Estado |
| created_at | TIMESTAMPTZ | Fecha de creación |
| updated_at | TIMESTAMPTZ | Fecha de actualización |

---

## Inventory

| Atributo | Tipo | Descripción |
|---|---|---|
| id | UUID (PK) | Identificador del inventario |
| product_id | UUID (FK) | Producto asociado |
| quantity | INTEGER | Cantidad disponible |
| min_stock | INTEGER | Stock mínimo |
| max_stock | INTEGER | Stock máximo |
| location | VARCHAR | Ubicación física |
| status | VARCHAR | Estado |
| created_at | TIMESTAMPTZ | Fecha de creación |
| updated_at | TIMESTAMPTZ | Fecha de actualización |

---

# 3. Billing

## Bill

| Atributo | Tipo | Descripción |
|---|---|---|
| id | UUID (PK) | Identificador de la factura |
| bill_number | VARCHAR | Número de factura |
| customer_id | UUID (FK) | Cliente (Person) |
| total_amount | NUMERIC | Total de la factura |
| status | VARCHAR | Estado |
| issue_date | DATE | Fecha de emisión |
| created_at | TIMESTAMPTZ | Fecha de creación |
| updated_at | TIMESTAMPTZ | Fecha de actualización |

---

## BillDetail

| Atributo | Tipo | Descripción |
|---|---|---|
| id | UUID (PK) | Identificador del detalle |
| bill_id | UUID (FK) | Factura asociada |
| product_id | UUID (FK) | Producto vendido |
| quantity | INTEGER | Cantidad |
| unit_price | NUMERIC | Precio unitario |
| subtotal | NUMERIC | Subtotal |
| created_at | TIMESTAMPTZ | Fecha de creación |
| updated_at | TIMESTAMPTZ | Fecha de actualización |

---

# Relaciones Principales

- Person **1 — 1** User  
- User **N — N** Role  
- Role **N — N** Permission  

- Category **1 — N** Product  
- Product **1 — 1** Inventory  

- Person **1 — N** Bill  
- Bill **1 — N** BillDetail  
- Product **1 — N** BillDetail

---