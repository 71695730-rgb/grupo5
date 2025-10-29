Diagramas del Sistema de Gestión de Procesos - Grupo 5
Tabla de Contenidos
Estructura General del Sistema
Diagrama de Flujo - Menú Principal
Diagrama de Flujo - Insertar Proceso
Diagrama de Flujo - Encolar Proceso
Diagrama de Flujo - Asignar Memoria
Estructura de Datos - Lista Enlazada
Estructura de Datos - Cola de Prioridad
Estructura de Datos - Pila (Memoria)
Casos de Uso del Sistema
1. Estructura General del Sistema
```mermaid
graph TD
    A[Sistema de Gestión de Procesos] --> B[Gestor de Procesos]
    A --> C[Planificador de CPU]
    A --> D[Gestor de Memoria]
    B --> B1[Lista Enlazada]
    B1 --> B2[Insertar Proceso]
    B1 --> B3[Eliminar Proceso]
    B1 --> B4[Buscar Proceso]
    B1 --> B5[Modificar Prioridad]
    C --> C1[Cola de Prioridad]
    C1 --> C2[Encolar Proceso]
    C1 --> C3[Desencolar Proceso]
    C1 --> C4[Visualizar Cola]
    D --> D1[Pila]
    D1 --> D2[Asignar Memoria - Push]
    D1 --> D3[Liberar Memoria - Pop]
    D1 --> D4[Verificar Estado]
Descripción: Este diagrama muestra la arquitectura general del sistema dividido en tres módulos principales.
    
2. Diagrama de Flujo - Menú Principal
mermaid
flowchart TD
    Start([Inicio]) --> Menu[Mostrar Menú Principal]
    Menu --> Input[Leer Opción del Usuario]
    Input --> Decision{Opción válida?}
    Decision -->|No| Error[Mostrar error]
    Error --> Menu
    Decision -->|Sí| Switch{Selección}
    Switch -->|1| GestProc[Gestor de Procesos]
    Switch -->|2| PlanCPU[Planificador CPU]
    Switch -->|3| GestMem[Gestor de Memoria]
    Switch -->|4| Guardar[Guardar Datos]
    Switch -->|5| Cargar[Cargar Datos]
    Switch -->|6| End([Salir])
    GestProc --> SubMenu1[Submenú Procesos]
    PlanCPU --> SubMenu2[Submenú CPU]
    GestMem --> SubMenu3[Submenú Memoria]
    Guardar --> GuardarArchivo[Persistir en archivo]
    Cargar --> CargarArchivo[Leer desde archivo]
    SubMenu1 --> Menu
    SubMenu2 --> Menu
    SubMenu3 --> Menu
    GuardarArchivo --> Menu
    CargarArchivo --> Menu
Descripción: Flujo principal del programa mostrando la navegación entre módulos.

3. Diagrama de Flujo - Insertar Proceso
mermaid
flowchart TD
    Start([Inicio: Insertar Proceso]) --> Input1[Solicitar ID del proceso]
    Input1 --> Input2[Solicitar nombre del proceso]
    Input2 --> Input3[Solicitar prioridad]
    Input3 --> Input4[Solicitar tiempo de ejecución]
    Input4 --> Validate{Datos válidos?}
    Validate -->|No| Error[Mostrar mensaje de error]
    Error --> Input1
    Validate -->|Sí| CheckEmpty{Lista vacía?}
    CheckEmpty -->|Sí| CreateFirst[Crear primer nodo]
    CreateFirst --> SetHead[Establecer como cabeza]
    SetHead --> Success
    CheckEmpty -->|No| CheckDuplicate{ID ya existe?}
    CheckDuplicate -->|Sí| ErrorDup[Error: ID duplicado]
    ErrorDup --> End2([Fin])
    CheckDuplicate -->|No| CreateNode[Crear nuevo nodo]
    CreateNode --> InsertPosition{Posición de inserción?}
    InsertPosition -->|Inicio| InsertStart[Insertar al inicio]
    InsertPosition -->|Final| InsertEnd[Insertar al final]
    InsertPosition -->|Medio| InsertMiddle[Insertar en posición]
    InsertStart --> Success[Proceso insertado exitosamente]
    InsertEnd --> Success
    InsertMiddle --> Success
    Success --> Display[Mostrar mensaje de éxito]
    Display --> End([Fin])
Descripción: Algoritmo para insertar un nuevo proceso en la lista enlazada con validaciones.

4. Diagrama de Flujo - Encolar Proceso
mermaid
flowchart TD
    Start([Inicio: Encolar Proceso]) --> Input[Solicitar ID del proceso]
    Input --> Search[Buscar proceso en lista de procesos]
    Search --> Exists{Proceso existe?}
    Exists -->|No| Error1[Error: Proceso no encontrado]
    Error1 --> End2([Fin])
    Exists -->|Sí| GetPriority[Obtener prioridad del proceso]
    GetPriority --> CheckQueue{Cola vacía?}
    CheckQueue -->|Sí| InsertFirst[Insertar como primer elemento]
    InsertFirst --> Success
    CheckQueue -->|No| Compare[Comparar prioridades]
    Compare --> FindPosition[Encontrar posición según prioridad]
    FindPosition --> InsertPos{Posición encontrada}
    InsertPos -->|Alta prioridad| InsertFront[Insertar al frente]
    InsertPos -->|Prioridad media| InsertMiddle[Insertar en medio]
    InsertPos -->|Baja prioridad| InsertRear[Insertar al final]
    InsertFront --> Success[Proceso encolado exitosamente]
    InsertMiddle --> Success
    InsertRear --> Success
    Success --> UpdateCount[Actualizar contador de cola]
    UpdateCount --> Display[Mostrar mensaje de éxito]
    Display --> End([Fin])
Descripción: Algoritmo para encolar procesos según su prioridad en la cola del planificador.

5. Diagrama de Flujo - Asignar Memoria
mermaid
flowchart TD
    Start([Inicio: Asignar Memoria]) --> Input[Solicitar ID del proceso]
    Input --> Input2[Solicitar tamaño de memoria]
    Input2 --> Validate{Datos válidos?}
    Validate -->|No| Error[Error: Datos inválidos]
    Error --> End2([Fin])
    Validate -->|Sí| CheckMemory{Memoria disponible?}
    CheckMemory -->|No| ErrorMem[Error: Memoria insuficiente]
    ErrorMem --> End2
    CheckMemory -->|Sí| CreateBlock[Crear bloque de memoria]
    CreateBlock --> Push[PUSH: Apilar bloque]
    Push --> UpdateTop[Actualizar tope de la pila]
    UpdateTop --> UpdateAvailable[Actualizar memoria disponible]
    UpdateAvailable --> SaveProcess[Asociar bloque con proceso]
    SaveProcess --> Success[Memoria asignada exitosamente]
    Success --> Display[Mostrar dirección de memoria]
    Display --> ShowStack[Mostrar estado de la pila]
    ShowStack --> End([Fin])
Descripción: Algoritmo para asignar memoria a un proceso usando operación PUSH en la pila.

6. Estructura de Datos - Lista Enlazada
mermaid
graph LR
    Head[Cabeza/Head] --> N1[Nodo 1<br/>ID: 101<br/>Nombre: Proceso A<br/>Prioridad: 3]
    N1 --> N2[Nodo 2<br/>ID: 102<br/>Nombre: Proceso B<br/>Prioridad: 1]
    N2 --> N3[Nodo 3<br/>ID: 103<br/>Nombre: Proceso C<br/>Prioridad: 2]
    N3 --> NULL[NULL]
    style Head fill:#FFE5B4
    style N1 fill:#B4D7FF
    style N2 fill:#B4D7FF
    style N3 fill:#B4D7FF
    style NULL fill:#FFB4B4
Descripción: Representación visual de la lista enlazada simple para gestión de procesos.

7. Estructura de Datos - Cola de Prioridad
mermaid
graph LR
    Front[Frente] --> P1[Proceso<br/>ID: 105<br/>Prioridad: 1<br/>ALTA]
    P1 --> P2[Proceso<br/>ID: 103<br/>Prioridad: 2<br/>MEDIA]
    P2 --> P3[Proceso<br/>ID: 107<br/>Prioridad: 3<br/>BAJA]
    P3 --> Rear[Final]
    Dequeue[Desencolar ←] -.-> Front
    Rear -.-> Enqueue[→ Encolar]
    style Front fill:#90EE90
    style P1 fill:#FF6B6B
    style P2 fill:#FFD93D
    style P3 fill:#6BCB77
    style Rear fill:#4D96FF
Descripción: Cola de prioridad donde los procesos se ordenan según su prioridad (1=alta, 3=baja).

8. Estructura de Datos - Pila (Memoria)
mermaid
graph TB
    Top[Tope/Top] --> B1[Bloque 3<br/>Proceso: 103<br/>Tamaño: 256 KB<br/>Dir: 0x3000]
    B1 --> B2[Bloque 2<br/>Proceso: 102<br/>Tamaño: 512 KB<br/>Dir: 0x2000]
    B2 --> B3[Bloque 1<br/>Proceso: 101<br/>Tamaño: 1024 KB<br/>Dir: 0x1000]
    B3 --> Bottom[Base]
    Pop[POP ↑<br/>Liberar] -.-> Top
    Bottom -.-> Push[↓ PUSH<br/>Asignar]
    style Top fill:#FF6B6B
    style B1 fill:#B4D7FF
    style B2 fill:#B4D7FF
    style B3 fill:#B4D7FF
    style Bottom fill:#90EE90
Descripción: Pila LIFO para gestión de bloques de memoria asignados a procesos.

9. Casos de Uso del Sistema
mermaid
graph TD
    Usuario([Usuario del Sistema])
    Usuario --> UC1[Agregar nuevo proceso]
    Usuario --> UC2[Eliminar proceso]
    Usuario --> UC3[Buscar proceso por ID]
    Usuario --> UC4[Modificar prioridad]
    Usuario --> UC5[Ejecutar proceso de cola]
    Usuario --> UC6[Ver cola de procesos]
    Usuario --> UC7[Asignar memoria]
    Usuario --> UC8[Liberar memoria]
    Usuario --> UC9[Ver estado de memoria]
    Usuario --> UC10[Guardar datos]
    Usuario --> UC11[Cargar datos]
    style Usuario fill:#FFE5B4
    style UC1 fill:#B4D7FF
    style UC2 fill:#B4D7FF
    style UC3 fill:#B4D7FF
    style UC4 fill:#B4D7FF
    style UC5 fill:#FFD93D
    style UC6 fill:#FFD93D
    style UC7 fill:#90EE90
    style UC8 fill:#90EE90
    style UC9 fill:#90EE90
    style UC10 fill:#DDA0DD
    style UC11 fill:#DDA0DD
Descripción: Casos de uso principales del sistema desde la perspectiva del usuario.

Notas de Implementación
Lista Enlazada (Gestor de Procesos)
Operaciones: Insertar, eliminar, buscar, modificar
Complejidad temporal: O(n) para búsqueda, O(1) para inserción al inicio
Uso: Mantener registro de todos los procesos del sistema
Cola de Prioridad (Planificador CPU)
Operaciones: Encolar según prioridad, desencolar, visualizar
Complejidad temporal: O(n) para encolar con orden, O(1) para desencolar
Uso: Simular ejecución de procesos según prioridad
Pila (Gestor de Memoria)
Operaciones: Push (asignar), Pop (liberar), peek (consultar)
Complejidad temporal: O(1) para todas las operaciones
Uso: Gestionar asignación y liberación de bloques de memoria
Cómo usar estos diagramas
Para el informe técnico: Exporta cada diagrama como imagen PNG desde GitHub
Para la presentación: Usa capturas de pantalla de los diagramas renderizados
Para el código: Estos diagramas sirven como guía para implementar las funciones
Proyecto: Sistema de Gestión de Procesos
Grupo: 5
Asignatura: Estructura de Datos
Fecha: 2025


