### Pseudocódigo: Agregar Proceso

```pseudocode
PROCEDIMIENTO agregarProceso(nombre, prioridad)
INICIO
    SI nombre está vacío O prioridad < 1 O prioridad > 5 ENTONCES
        Mostrar "Error: Datos inválidos"
        RETORNAR
    FIN SI
    
    SI buscarProcesoPorNombre(nombre) NO es NULL ENTONCES
        Mostrar "Error: Ya existe un proceso con ese nombre"
        RETORNAR
    FIN SI
    
    nuevo = crear nuevo nodo de Proceso
    nuevo.id = generarIDUnico()
    copiar nombre a nuevo.nombre
    nuevo.prioridad = prioridad
    copiar "NUEVO" a nuevo.estado
    nuevo.siguiente = NULL
    
    SI cabeza es NULL ENTONCES
        cabeza = nuevo
        Mostrar "Proceso creado como primero en la lista"
    SINO
        actual = cabeza
        MIENTRAS actual.siguiente NO sea NULL HACER
            actual = actual.siguiente
        FIN MIENTRAS
        actual.siguiente = nuevo
        Mostrar "Proceso agregado al final"
    FIN SI
    
    Mostrar "Proceso registrado exitosamente con ID: " concatenar nuevo.id
    
    Mostrar "¿Desea agregarlo a la cola de ejecución? (S/N)"
    Leer respuesta
    SI respuesta es 'S' ENTONCES
        cambiarEstado(nuevo.id, "LISTO")
        encolarProceso(nuevo)
    FIN SI
FIN
```

### Pseudocódigo: Cambiar Estado del Proceso
```pseudocode
PROCEDIMIENTO cambiarEstado(idProceso, nuevoEstado)
INICIO
    proceso = buscarProceso(idProceso)
    
    SI proceso es NULL ENTONCES
        Mostrar "Error: Proceso no encontrado"
        RETORNAR falso
    FIN SI
    
    estadoAnterior = proceso.estado
    transicionValida = falso
    
    SI estadoAnterior es "NUEVO" Y nuevoEstado es "LISTO" ENTONCES
        transicionValida = verdadero
    FIN SI
    
    SI estadoAnterior es "LISTO" Y nuevoEstado es "EJECUTANDO" ENTONCES
        transicionValida = verdadero
    FIN SI
    
    SI estadoAnterior es "EJECUTANDO" ENTONCES
        SI nuevoEstado es "LISTO" O nuevoEstado es "TERMINADO" ENTONCES
            transicionValida = verdadero
        FIN SI
    FIN SI
    
    SI NO transicionValida ENTONCES
        Mostrar "Error: Transición no permitida de " concatenar estadoAnterior 
                concatenar " a " concatenar nuevoEstado
        RETORNAR falso
    FIN SI
    
    copiar nuevoEstado a proceso.estado
    Mostrar "Estado cambiado de " concatenar estadoAnterior 
            concatenar " a " concatenar nuevoEstado
    
    SI nuevoEstado es "LISTO" ENTONCES
        encolarProceso(proceso)
        Mostrar "Proceso agregado a la cola de ejecución"
    FIN SI
    
    SI nuevoEstado es "TERMINADO" ENTONCES
        liberarMemoriaProceso(idProceso)
        Mostrar "Memoria del proceso liberada"
    FIN SI
    
   RETORNAR verdadero
FIN
```
