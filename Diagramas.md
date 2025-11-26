# Sistema de Gestión y Reducción del Desperdicio de Alimentos

## 1. Diagrama de Contexto (PlantUML)

```plantuml
@startuml
!define RECTANGLE class

skinparam rectangle {
    BackgroundColor LightBlue
    BorderColor Black
}

rectangle "Sistema de Gestión\nde Donaciones" as Sistema

actor "Donante" as Donante
actor "Administrador" as Admin
actor "Beneficiario" as Beneficiario
actor "Centro de Acopio" as Centro

Donante --> Sistema : Registra donaciones
Admin --> Sistema : Gestiona inventario\ny distribución
Beneficiario --> Sistema : Consulta entregas
Centro --> Sistema : Controla stock

Sistema --> Donante : Envía constancias
Sistema --> Admin : Genera reportes\ny alertas
Sistema --> Beneficiario : Notifica entregas
Sistema --> Centro : Alertas de vencimiento

@enduml
```

## 2. Diagrama de Casos de Uso (PlantUML)

```plantuml
@startuml
left to right direction
skinparam packageStyle rectangle

actor "Donante" as Donante
actor "Administrador" as Admin
actor "Beneficiario" as Beneficiario
actor "Centro de Acopio" as Centro

rectangle "Sistema de Gestión de Donaciones" {
  usecase "Registrar Donación" as UC1
  usecase "Consultar Historial" as UC2
  usecase "Descargar Constancia" as UC3
  usecase "Gestionar Inventario" as UC4
  usecase "Asignar Distribución" as UC5
  usecase "Generar Reportes" as UC6
  usecase "Recibir Alertas" as UC7
  usecase "Validar Donaciones" as UC8
  usecase "Consultar Entregas" as UC9
  usecase "Confirmar Recepción" as UC10
  usecase "Controlar Stock" as UC11
}

Donante --> UC1
Donante --> UC2
Donante --> UC3

Admin --> UC4
Admin --> UC5
Admin --> UC6
Admin --> UC7
Admin --> UC8

Beneficiario --> UC9
Beneficiario --> UC10

Centro --> UC11
Centro --> UC7

@enduml
```

## 3. Diagrama de Actividad (PlantUML)

```plantuml
@startuml
start
:Usuario ingresa al sistema;
:Iniciar sesión;

if (¿Credenciales válidas?) then (sí)
  :Acceder al panel según rol;
  
  if (¿Tipo de usuario?) then (Donante)
    :Mostrar panel de donante;
    :Registrar nueva donación;
    :Sistema valida datos;
    :Guardar en base de datos;
    :Generar constancia digital;
  elseif (Administrador) then
    :Mostrar panel administrativo;
    :Consultar inventario;
    if (¿Productos por vencer?) then (sí)
      :Mostrar alerta;
      :Planificar distribución urgente;
    endif
    :Asignar alimentos a beneficiarios;
    :Registrar distribución;
    :Actualizar inventario;
  else (Beneficiario)
    :Mostrar panel de beneficiario;
    :Consultar entregas programadas;
    :Confirmar recepción;
    :Actualizar historial;
  endif
  
  :Sistema genera notificación;
else (no)
  :Mostrar error de autenticación;
endif

stop
@enduml
```

## 4. Diagrama de Secuencia - Registrar Donación (PlantUML)

```plantuml
@startuml
actor Donante
participant "Sistema Web" as Web
participant "API REST" as API
participant "Lógica de Negocio" as Logic
database "Base de Datos" as DB

Donante -> Web: Accede al formulario
Web -> Donante: Muestra formulario vacío

Donante -> Web: Ingresa datos de donación
Web -> API: POST /api/donaciones
API -> Logic: validarDonacion(datos)

alt Datos válidos
    Logic -> DB: INSERT INTO Donacion
    DB -> Logic: OK (id generado)
    Logic -> DB: INSERT INTO Inventario
    DB -> Logic: OK
    Logic -> API: {status: 200, id: 123}
    API -> Web: Donación registrada
    Web -> Donante: Muestra constancia digital
    
    Logic -> Logic: generarNotificacion()
    Logic -> Web: Envía alerta a administrador
else Datos inválidos
    Logic -> API: {status: 400, error}
    API -> Web: Error de validación
    Web -> Donante: Muestra mensaje de error
end

@enduml
```

## 5. Diagrama de Clases (PlantUML)

```plantuml
@startuml
class Usuario {
  - id: int
  - nombre: string
  - email: string
  - password_hash: string
  - rol: string
  - fecha_registro: date
  + login()
  + logout()
  + cambiarPassword()
}

class Donante {
  - tipo: string
  - contacto: string
  - direccion: string
  + registrarDonacion()
  + consultarHistorial()
  + descargarConstancia()
}

class Administrador {
  - permisos: string[]
  + gestionarInventario()
  + asignarDistribucion()
  + generarReportes()
  + validarDonaciones()
}

class Beneficiario {
  - tipo: string
  - direccion: string
  - num_integrantes: int
  + consultarEntregas()
  + confirmarRecepcion()
}

class Donacion {
  - id: int
  - id_donante: int
  - fecha: date
  - tipo_producto: string
  - cantidad: int
  - estado: string
  + registrar()
  + validar()
  + obtenerConstancia()
}

class Inventario {
  - id: int
  - id_donacion: int
  - lote: string
  - fecha_ingreso: date
  - fecha_caducidad: date
  - cantidad_disponible: int
  + agregarProducto()
  + actualizarStock()
  + verificarVencimiento()
  + obtenerDisponibles()
}

class Distribucion {
  - id: int
  - id_inventario: int
  - id_beneficiario: int
  - fecha_entrega: date
  - cantidad: int
  - estado: string
  + asignar()
  + confirmar()
  + cancelar()
}

class Alerta {
  - id: int
  - tipo: string
  - mensaje: string
  - fecha: datetime
  - leida: boolean
  + crear()
  + marcarLeida()
  + obtenerPendientes()
}

Usuario <|-- Donante
Usuario <|-- Administrador
Usuario <|-- Beneficiario

Donante "1" -- "0..*" Donacion : realiza
Donacion "1" -- "1" Inventario : genera
Inventario "1" -- "0..*" Distribucion : origina
Beneficiario "1" -- "0..*" Distribucion : recibe
Inventario "1" -- "0..*" Alerta : genera

@enduml
```

## 6. Modelo Entidad-Relación (Mermaid)

```mermaid
erDiagram
    USUARIO ||--o{ DONANTE : es
    USUARIO ||--o{ ADMINISTRADOR : es
    USUARIO ||--o{ BENEFICIARIO : es
    
    DONANTE ||--o{ DONACION : realiza
    DONACION ||--|| INVENTARIO : genera
    INVENTARIO ||--o{ DISTRIBUCION : origina
    BENEFICIARIO ||--o{ DISTRIBUCION : recibe
    INVENTARIO ||--o{ ALERTA : genera
    
    USUARIO {
        int id PK
        string nombre
        string email UK
        string password_hash
        string rol
        date fecha_registro
    }
    
    DONANTE {
        int id PK
        int id_usuario FK
        string tipo
        string contacto
        string direccion
    }
    
    DONACION {
        int id PK
        int id_donante FK
        date fecha
        string tipo_producto
        int cantidad
        string estado
    }
    
    INVENTARIO {
        int id PK
        int id_donacion FK
        string lote
        date fecha_ingreso
        date fecha_caducidad
        int cantidad_disponible
        string estado
    }
    
    BENEFICIARIO {
        int id PK
        int id_usuario FK
        string tipo
        string direccion
        int num_integrantes
    }
    
    DISTRIBUCION {
        int id PK
        int id_inventario FK
        int id_beneficiario FK
        date fecha_entrega
        int cantidad
        string estado
        datetime fecha_confirmacion
    }
    
    ALERTA {
        int id PK
        int id_inventario FK
        string tipo
        string mensaje
        datetime fecha
        boolean leida
    }
```

## 7. Diagrama de Arquitectura (Mermaid)

```mermaid
graph TB
    subgraph "Capa de Presentación"
        A[Web App - React/HTML]
        B[Mobile App - Flutter]
    end
    
    subgraph "Capa de Lógica de Negocio"
        C[API REST - Node.js/Python]
        D[Autenticación JWT]
        E[Validación de Datos]
        F[Lógica de Donaciones]
        G[Lógica de Inventario]
        H[Lógica de Distribución]
        I[Generador de Reportes]
    end
    
    subgraph "Capa de Datos"
        J[(Base de Datos PostgreSQL/MySQL)]
        K[Procedimientos Almacenados]
        L[Triggers]
        M[Vistas]
    end
    
    A --> C
    B --> C
    C --> D
    C --> E
    C --> F
    C --> G
    C --> H
    C --> I
    D --> J
    F --> J
    G --> J
    H --> J
    I --> J
    J --> K
    J --> L
    J --> M
```

## 8. Flujo de Navegación (Mermaid)

```mermaid
graph TD
    A[Pantalla de Inicio] --> B{Iniciar Sesión}
    A --> C[Registrar Cuenta]
    
    B --> D{Tipo de Usuario}
    
    D -->|Donante| E[Panel del Donante]
    D -->|Administrador| F[Panel del Administrador]
    D -->|Beneficiario| G[Panel del Beneficiario]
    
    E --> E1[Registrar Donación]
    E --> E2[Ver Historial]
    E --> E3[Descargar Constancias]
    E --> E4[Notificaciones]
    
    F --> F1[Gestión de Inventario]
    F --> F2[Gestión de Donaciones]
    F --> F3[Gestión de Beneficiarios]
    F --> F4[Reportes y Estadísticas]
    
    F1 --> F1A[Lista de Productos]
    F1 --> F1B[Nuevo Producto]
    F1 --> F1C[Alertas de Vencimiento]
    
    F1B --> F1B1[Información Básica]
    F1B1 --> F1B2[Datos de Inventario]
    F1B2 --> F1B3[Guardar Producto]
    
    G --> G1[Ver Entregas Programadas]
    G --> G2[Confirmar Recepción]
    G --> G3[Notificaciones]
    
    C --> C1[Completar Formulario]
    C1 --> C2[Enviar Solicitud]
```

## 9. Diagrama de Despliegue (Mermaid)

```mermaid
graph TB
    subgraph "Cliente"
        A[Navegador Web]
        B[App Móvil]
    end
    
    subgraph "Servidor Web - HTTPS"
        C[Nginx/Apache]
        D[Balanceador de Carga]
    end
    
    subgraph "Servidor de Aplicación"
        E[Node.js/Python Server]
        F[API REST]
        G[Lógica de Negocio]
    end
    
    subgraph "Servidor de Base de Datos"
        H[(PostgreSQL/MySQL)]
        I[Réplica Lectura]
    end
    
    subgraph "Servicios Externos"
        J[Servicio de Email]
        K[Notificaciones Push]
    end
    
    A -->|HTTPS| C
    B -->|HTTPS| C
    C --> D
    D --> E
    E --> F
    F --> G
    G --> H
    H --> I
    G --> J
    G --> K
```

## 10. Diagrama de Componentes (Mermaid)

```mermaid
graph LR
    subgraph "Frontend"
        A[Componente Login]
        B[Componente Dashboard]
        C[Componente Donaciones]
        D[Componente Inventario]
        E[Componente Distribución]
    end
    
    subgraph "Backend API"
        F[Auth Controller]
        G[Donaciones Controller]
        H[Inventario Controller]
        I[Distribución Controller]
        J[Reportes Controller]
    end
    
    subgraph "Servicios"
        K[Auth Service]
        L[Donaciones Service]
        M[Inventario Service]
        N[Distribución Service]
        O[Email Service]
    end
    
    subgraph "Repositorios"
        P[Usuario Repository]
        Q[Donación Repository]
        R[Inventario Repository]
        S[Distribución Repository]
    end
    
    A --> F
    C --> G
    D --> H
    E --> I
    
    F --> K
    G --> L
    H --> M
    I --> N
    
    K --> P
    L --> Q
    M --> R
    N --> S
    
    L --> O
    M --> O
    N --> O
```

---

## Instrucciones para usar estos diagramas:

### PlantUML
1. Copia el código y guárdalo con extensión `.puml`
2. Usa [PlantUML Online](http://www.plantuml.com/plantuml/uml/) o instala la extensión en VS Code
3. Genera las imágenes en formato PNG o SVG

### Mermaid
1. Copia el código en archivos `.md` o `.mmd`
2. GitHub renderiza automáticamente los diagramas Mermaid
3. También puedes usar [Mermaid Live Editor](https://mermaid.live/)

### Estructura recomendada para GitHub:
```
proyecto/
├── docs/
│   ├── diagramas/
│   │   ├── README.md (este archivo)
│   │   ├── contexto.puml
│   │   ├── casos-uso.puml
│   │   ├── actividad.puml
│   │   ├── secuencia.puml
│   │   ├── clases.puml
│   │   ├── er-diagram.mmd
│   │   ├── arquitectura.mmd
│   │   ├── navegacion.mmd
│   │   ├── despliegue.mmd
│   │   └── componentes.mmd
│   └── images/
│       └── (imágenes generadas)
└── README.md
```
