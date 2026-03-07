# Resumen: Patrones Arquitectónicos y Diseño de Software

## Fecha: Semana 1 - Sesión 2

---

## 1. Conceptos Fundamentales

### 1.1 Patrones vs Plantilla vs Arquitectura

- **Patrones**: Prácticas basadas en conocimiento previo que resuelven problemas comunes de diseño
- **Plantilla**: Estructura rígida predefinida que se debe seguir
- **Arquitectura**: El arte de construir software, combinando:
  - **Hardware**: Híbrida, en casa (on-premise), o en nube
  - **Software**: Stack tecnológico (BD, frameworks, lenguajes)
  - **Metodologías**: Ágil (Scrum) o Tradicional (Cascada)

---

## 2. Patrón MVC (Model-View-Controller)

### 2.1 Estructura Básica del Patrón MVC

El patrón MVC es uno de los patrones arquitectónicos clásicos más utilizados en el desarrollo de software cliente-servidor.

#### Componentes Principales:

1. **Model (Modelo)**
   - Archivos de abstracción de negocio
   - Representa la estructura de datos
   - Diagrama de clases

2. **Controller (Controlador)**
   - Reglas de negocio
   - Operaciones CRUD (Create, Read, Update, Delete)
   - Generación de reportes
   - Implementación mediante switch-case o estrategias similares

3. **View (Vista)**
   - Capa de visualización
   - Formularios
   - Landing pages
   - Interfaces de terminal/consola

4. **Punto de Entrada**
   - `index.[java, php, c#]`

### 2.2 MVC Extendido con Utilidades y Adaptadores

```
Software cliente-servidor nativo (Frontend + Backend)
│
├── utils/
│   └── Caja de herramientas (validaciones, helpers)
│
├── adapter/
│   └── Configuración en XML (integraciones externas)
│
├── model/
│   └── Abstracción de negocio (clases de dominio)
│
├── controller/
│   └── Reglas de negocio (CRUD y reportes)
│
├── view/
│   └── Capa de visualización (UI)
│
└── index.[java, php, c#]
```

---

## 3. Ejemplos Prácticos de MVC

### 3.1 Escenario 1: Aplicación Monolítica (Frontend + Backend Integrado)

**Sistema de Facturación e Inventario**

```
├── utils/
│   └── Validaciones
│
├── adapter/
│   └── Facturación electrónica (integración DIAN)
│
├── model/
│   ├── Facturación
│   └── Inventario
│
├── controller/
│   ├── Facturación
│   └── Inventario
│
├── view/
│   ├── Facturación
│   └── Inventario
│
└── index.[java, php, c#]
```

**Características:**
- Todo el código (frontend y backend) en un solo proyecto
- Despliegue unificado
- Ideal para aplicaciones pequeñas o medianas

### 3.2 Escenario 2: Aplicación Distribuida (Frontend Separado - Backend API)

**Sistema con Separación Frontend-Backend**

#### Backend (AWS)
```
├── utils/
│   └── Validaciones
│
├── adapter/
│   └── Facturación electrónica
│
├── model/
│   ├── Facturación
│   └── Inventario
│
└── controller/
    ├── Facturación
    └── Inventario
```

#### Frontend (Hosting)
```
├── router/
│   └── Gestión de rutas
│
├── service/
│   └── Consumo de API
│
├── view/
│   ├── Facturación
│   └── Inventario
│
└── index.[html, jsx, vue]
```

**Ventajas:**
- Escalabilidad independiente
- Posibilidad de múltiples frontends (web, móvil)
- Despliegue separado
- Optimización de recursos cloud

---

## 4. Arquitectura N-Capas

### 4.1 Organización por Proyecto Completo (Enfoque Global)

Para una API que gestiona Security, Inventory y Billing:

```
API Project
│
├── Entity/
│   ├── Security
│   ├── Inventory
│   └── Billing
│
├── DAO/ (Data Access Object)
│   ├── SecurityDAO
│   ├── InventoryDAO
│   └── BillingDAO
│
├── DTO/ (Data Transfer Object)
│   ├── Security
│   ├── Inventory
│   └── Billing
│
├── IService/ (Interfaces de Servicio)
│   ├── ISecurity
│   ├── IInventory
│   └── IBilling
│
├── Service/ (Implementación de Servicios)
│   ├── SecurityService
│   ├── InventoryService
│   └── BillingService
│
├── Controller/
│   ├── SecurityController
│   ├── InventoryController
│   └── BillingController
│
└── Utils/
    ├── Validaciones JWT
    ├── Validaciones de fecha
    ├── Validaciones de idioma
    └── Validaciones de Stock
```

**Módulos del Sistema:**
- **Security**: Person, User, Role, Permission
- **Inventory**: Product, Category, Inventory
- **Billing**: Bill, BillDetail

### 4.2 Organización por Módulo (Enfoque Modular)

Cada módulo contiene su propia estructura completa de capas.

#### Módulo Security
```
Security/
├── Entity
├── DAO
├── DTO
├── IService
├── Service
├── Controller
├── Utils/
│   ├── Validaciones JWT
│   └── Validaciones de idioma
└── Security/
    └── Anotaciones de seguridad
```

**Responsabilidades:**
- Gestión de autenticación y autorización
- Manejo de tokens JWT
- Control de acceso basado en roles
- Validaciones de idioma/localización

#### Módulo Inventory
```
Inventory/
├── Entity
├── DAO
├── DTO
├── IService
├── Service
├── Controller
├── Utils/
│   └── Swagger (documentación API)
└── Adapter/
    ├── CRM (integración con sistemas CRM)
    └── Validaciones de Stock
```

**Responsabilidades:**
- Gestión de productos y categorías
- Control de inventario
- Integración con sistemas externos (CRM)
- Validación de existencias

#### Módulo Billing
```
Billing/
├── Entity
├── DAO
├── DTO
├── IService
├── Service
├── Controller
└── Adapter/
    └── Conexiones API DIAN (facturación electrónica)
```

**Responsabilidades:**
- Emisión de facturas
- Detalles de facturación
- Integración con sistemas gubernamentales (DIAN)
- Cumplimiento normativo

#### Módulo Utils (Compartido)
```
Utils/
└── Validaciones de fecha
```

**Ventajas del Enfoque Modular:**
- ✅ Alta cohesión dentro de cada módulo
- ✅ Bajo acoplamiento entre módulos
- ✅ Fácil mantenimiento
- ✅ Escalabilidad
- ✅ Reutilización de código

**Desventajas del Enfoque Modular:**
- ❌ Mayor complejidad inicial
- ❌ Curva de aprendizaje más pronunciada
- ❌ Requiere más planificación

---

## 5. Arquitectura de Software

### 5.1 Componentes de la Arquitectura

La arquitectura de software es el arte de construir aplicaciones considerando múltiples factores:

#### Hardware
- **Híbrida**: Combinación de on-premise y nube
- **En Casa (On-Premise)**: Infraestructura local
- **En Nube**: AWS, Azure, GCP

#### Software - Stack Tecnológico
**Selección de Base de Datos:**
- Oracle ❌ (costoso, licenciamiento)
- PostgreSQL ✅ (open source, robusto)
- MongoDB ✅ (NoSQL, flexible, escalable)

**Consideraciones:**
- Costo de licenciamiento
- Escalabilidad
- Soporte de la comunidad
- Compatibilidad con el stack

#### Metodologías
- **Ágil (Scrum)**: Iterativo, entregas frecuentes, adaptable
- **Tradicional (Cascada)**: Secuencial, documentación exhaustiva

### 5.2 Pregunta Clave: ¿El Sistema Puede Crecer?

La arquitectura debe diseñarse pensando en:
- Escalabilidad horizontal y vertical
- Modularidad
- Mantenibilidad
- Extensibilidad

---

## 6. Gestión de Repositorios y Ambientes

### 6.1 Estrategia de Branching

```
main (producción)
  ↑
  |
qa (quality assurance)
  ↑
  |
develop (desarrollo)
  ↑
  |
feature/HU-xxx (historias de usuario)
```

### 6.2 Reglas de Gestión de Ramas

1. **Los ambientes padres nunca se alteran directamente**
   - No commits directos a `main`, `qa`, o `develop`
   - Solo mediante Pull Requests

2. **Roles de la empresa y control de asignación**
   - Permisos basados en roles
   - Aprobaciones requeridas para merge

3. **Cada cambio se realiza mediante una Historia de Usuario (HU)**
   - Rama creada desde su rama padre
   - Nomenclatura: `feature/HU-xxx`, `bugfix/HU-xxx`, `hotfix/HU-xxx`
   - Solo puede afectar a su rama padre

### 6.3 Flujo de Trabajo

```
1. Crear rama desde develop: feature/HU-123
2. Desarrollar y commit en feature/HU-123
3. PR de feature/HU-123 → develop
4. PR de develop → qa (después de testing)
5. PR de qa → main (después de QA approval)
```

**Beneficios:**
- Trazabilidad completa
- Prevención de errores en producción
- Control de calidad en cada etapa
- Rollback sencillo si es necesario

---

## 7. Frameworks y Estándares

### 7.1 Normas ISO y Frameworks de Gestión

- **ISO 27000.1**: Seguridad de la información
- **ISO 31000.1**: Gestión de riesgos
- **ISO 9000.1**: Gestión de calidad
- **Magerit**: Metodología de análisis y gestión de riesgos
- **COBIT**: Framework de gobierno y gestión de TI
- **CMMI**: Modelo de madurez y capacidad integrado

### 7.2 Gestión de Procesos de Negocio

- **BPM** (Business Process Management): Gestión de procesos de negocio
- **BPMN** (Business Process Model and Notation): Notación para modelado de procesos

---

## 8. Proveedores de Nube

### Principales Proveedores
- **AWS** (Amazon Web Services)
- **Azure** (Microsoft)
- **GCP** (Google Cloud Platform)

**Consideraciones de selección:**
- Costo
- Servicios disponibles
- Región geográfica
- Soporte técnico
- Integración con stack actual

---

## 9. Acoplamiento vs Cohesión

### 9.1 Definiciones

#### Acoplamiento
**Definición**: Grado de interdependencia entre módulos o componentes del software.

**Tipos de Acoplamiento (del peor al mejor):**

1. **Acoplamiento de Contenido** (Peor)
   - Un módulo modifica directamente datos internos de otro módulo

2. **Acoplamiento Común**
   - Módulos comparten datos globales

3. **Acoplamiento de Control**
   - Un módulo controla el flujo de otro mediante flags

4. **Acoplamiento de Datos**
   - Módulos comparten datos mediante parámetros

5. **Acoplamiento de Mensaje** (Mejor)
   - Comunicación mediante interfaces bien definidas

#### Cohesión
**Definición**: Grado en que los elementos de un módulo están relacionados funcionalmente.

**Tipos de Cohesión (del peor al mejor):**

1. **Cohesión Coincidental** (Peor)
   - Elementos agrupados sin relación lógica

2. **Cohesión Lógica**
   - Elementos que realizan actividades similares

3. **Cohesión Temporal**
   - Elementos ejecutados al mismo tiempo

4. **Cohesión Procedural**
   - Elementos que siguen una secuencia

5. **Cohesión Comunicacional**
   - Elementos operan sobre los mismos datos

6. **Cohesión Secuencial**
   - La salida de un elemento es entrada del siguiente

7. **Cohesión Funcional** (Mejor)
   - Todos los elementos contribuyen a una única función

### 9.2 Ejemplos Prácticos

#### Ejemplo de Alto Acoplamiento (❌ Malo)

```java
// Clase Usuario accede directamente a detalles de Factura
public class Usuario {
    public void procesarCompra() {
        Factura factura = new Factura();
        factura.items.add(new Item()); // Acceso directo a internals
        factura.total = calcularTotal(); // Modificación directa
        factura.guardarEnBaseDatos(); // Dependencia directa de BD
    }
}
```

**Problemas:**
- ❌ Cambios en `Factura` afectan a `Usuario`
- ❌ Difícil de testear
- ❌ No reutilizable
- ❌ Rompe encapsulamiento

#### Ejemplo de Bajo Acoplamiento (✅ Bueno)

```java
// Comunicación mediante interfaces
public class Usuario {
    private IFacturacionService facturacionService;
    
    public Usuario(IFacturacionService service) {
        this.facturacionService = service;
    }
    
    public void procesarCompra(List<Item> items) {
        FacturaDTO factura = facturacionService.crearFactura(items);
        facturacionService.guardar(factura);
    }
}
```

**Ventajas:**
- ✅ Independencia entre módulos
- ✅ Fácil de testear (mocking)
- ✅ Reutilizable
- ✅ Respeta SOLID

#### Ejemplo de Baja Cohesión (❌ Malo)

```java
public class UtilidadesGenerales {
    public void enviarEmail() { }
    public void calcularImpuestos() { }
    public void validarPassword() { }
    public void generarReporte() { }
    public void conectarBaseDatos() { }
}
```

**Problemas:**
- ❌ Funciones no relacionadas
- ❌ Difícil de mantener
- ❌ Responsabilidades mezcladas
- ❌ Viola Single Responsibility Principle

#### Ejemplo de Alta Cohesión (✅ Bueno)

```java
// Módulo Security con alta cohesión
public class SecurityService {
    public boolean validarPassword(String password) { }
    public String encriptarPassword(String password) { }
    public boolean verificarCredenciales(String user, String pass) { }
    public Token generarToken(Usuario usuario) { }
}

// Módulo Email con alta cohesión
public class EmailService {
    public void enviarEmail(String destinatario, String mensaje) { }
    public void enviarEmailMasivo(List<String> destinatarios) { }
    public void configurarServidor(ConfigEmail config) { }
}
```

**Ventajas:**
- ✅ Funciones relacionadas agrupadas
- ✅ Fácil de entender
- ✅ Fácil de mantener
- ✅ Responsabilidad clara

### 9.3 Comparativa: Acoplamiento vs Cohesión

| Aspecto | Acoplamiento | Cohesión |
|---------|--------------|----------|
| **Objetivo** | Minimizar (Bajo) | Maximizar (Alta) |
| **Scope** | Entre módulos | Dentro de un módulo |
| **Objetivo** | Independencia | Responsabilidad única |
| **Mejor práctica** | Bajo acoplamiento | Alta cohesión |

### 9.4 Ventajas y Desventajas

#### Bajo Acoplamiento + Alta Cohesión (✅ Ideal)

**Ventajas:**
- ✅ Mantenibilidad mejorada
- ✅ Testabilidad sencilla
- ✅ Reutilización de código
- ✅ Escalabilidad
- ✅ Menor propagación de errores
- ✅ Desarrollo paralelo facilitado

**Desventajas:**
- ❌ Requiere más planificación inicial
- ❌ Puede aumentar el número de clases/módulos
- ❌ Curva de aprendizaje para el equipo

#### Alto Acoplamiento + Baja Cohesión (❌ Evitar)

**Ventajas:**
- ✅ Desarrollo inicial más rápido (aparentemente)
- ✅ Menos abstracción (más directo)

**Desventajas:**
- ❌ Código espagueti
- ❌ Difícil de mantener
- ❌ Testing complejo
- ❌ Cambios en cascada
- ❌ No escalable
- ❌ Alta deuda técnica

### 9.5 Principios SOLID Relacionados

1. **S - Single Responsibility Principle**: Alta cohesión
2. **O - Open/Closed Principle**: Bajo acoplamiento
3. **L - Liskov Substitution Principle**: Bajo acoplamiento
4. **I - Interface Segregation Principle**: Alta cohesión
5. **D - Dependency Inversion Principle**: Bajo acoplamiento

---

## 10. Conclusiones y Mejores Prácticas

### Diseño de Arquitectura
1. **Pensar en escalabilidad desde el inicio**
2. **Separar responsabilidades claramente**
3. **Usar patrones establecidos (MVC, N-Capas)**
4. **Mantener bajo acoplamiento y alta cohesión**

### Organización de Código
1. **Modular el código por dominio/contexto**
2. **Utilizar inyección de dependencias**
3. **Crear interfaces para abstracción**
4. **Documentar decisiones arquitectónicas**

### Gestión de Repositorio
1. **Nunca commits directos a ramas principales**
2. **Cada cambio mediante HU (Historia de Usuario)**
3. **Revisiones de código obligatorias**
4. **Pruebas automatizadas en cada ambiente**

### Selección de Tecnología
1. **Evaluar costo total de propiedad**
2. **Considerar la curva de aprendizaje del equipo**
3. **Verificar soporte y comunidad**
4. **Planificar migración futura**

---

## 11. Recursos Adicionales

- Frameworks de gestión: ISO 27000, COBIT, CMMI
- Metodologías: BPM, BPMN
- Cloud providers: AWS, Azure
- Patrones de diseño: Gang of Four
- Arquitectura limpia: Robert C. Martin

---

**Notas Importantes:**
- Este resumen cubre patrones arquitectónicos fundamentales
- Los ejemplos son adaptables a diferentes lenguajes y frameworks
- La selección de arquitectura depende del contexto del proyecto
- Siempre validar decisiones con el equipo y stakeholders