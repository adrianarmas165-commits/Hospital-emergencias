# Sistema de Emergencias del Hospital

Programa en Java que organiza la fila de espera de una sala de emergencias. Los pacientes se ordenan según su nivel de prioridad.

## 1. Problema

En una sala de emergencias no se atiende por orden de llegada, sino por gravedad. Cada paciente tiene una prioridad del 1 al 5:

- **1:** muy urgente
- **5:** poco urgente

El programa debe:

1. Guardar a los pacientes ordenados por prioridad. Si dos tienen la misma, va primero el que llegó antes.
2. Atender al primer paciente de la fila.
3. Mostrar a los pacientes que llevan mucho tiempo esperando.
4. Ordenar la fila por tiempo de espera, de mayor a menor.
5. Mostrar la lista de pacientes.

## 2. Estructura de datos

Se usa una **lista doblemente enlazada** implementada manualmente, sin usar `LinkedList` de Java.

Cada nodo (`NodoDoble`) guarda:

- `dato`: el paciente
- `siguiente`: el nodo de adelante
- `anterior`: el nodo de atrás

La clase `HospitalEmergency` guarda el `primero` y el `tamanio`.

```
null <- [Pedro] <-> [Juan] <-> [Ana] -> null
         primero
```

Se eligió porque el paciente más urgente siempre queda al inicio y atenderlo es rápido. Para insertar solo hay que cambiar los enlaces de los nodos vecinos, sin mover a los demás pacientes.

## 3. Archivos

| Archivo | Descripción |
|---------|-------------|
| `src/Paciente.java` | Datos del paciente: historial, nombre, prioridad y minutos de espera |
| `src/NodoDoble.java` | Nodo de la lista |
| `src/HospitalEmergency.java` | La lista y sus métodos |
| `src/Main.java` | Ejemplo con 4 pacientes |
| `src/Pruebas.java` | Casos de prueba |

## 4. Métodos

| Método | Qué hace |
|--------|----------|
| `insertarPorPrioridad(Paciente)` | Inserta al paciente según su prioridad |
| `atenderSiguientePaciente()` | Saca al primer paciente; devuelve `null` si no hay nadie |
| `pacientesEnEsperaCritica(int)` | Muestra a los que esperan más de los minutos indicados |
| `reordenarPorTiempoEspera()` | Ordena de mayor a menor tiempo de espera |
| `mostrarLista()` | Imprime la lista completa |

## 5. Cómo ejecutarlo

Se necesita tener el JDK instalado. Desde la carpeta del proyecto:

```
mkdir -p out
javac -d out src/*.java
java -cp out Main
java -cp out Pruebas
```

## 6. Pruebas realizadas

Todas se ejecutaron con `Pruebas.java` y dieron el resultado esperado.

| N.º | Prueba | Resultado esperado | Resultado |
|-----|--------|--------------------|-----------|
| 1 | Atender con la lista vacía | Mensaje "No hay pacientes en espera." y retorno `null` | Correcto |
| 2 | Un solo paciente: insertarlo y atenderlo | Aparece y luego la lista queda vacía | Correcto |
| 3 | Tres pacientes con la misma prioridad (Carla, Diego, Elena) | Orden de llegada | Correcto |
| 4 | Insertar al inicio, en medio y al final (prioridades 3, 1, 5, 4) | Orden 1, 3, 4, 5 | Correcto |
| 5 | Espera crítica con límite de 100 min, sin pacientes que lo superen | Mensaje "No hay pacientes en espera crítica." | Correcto |
| 6 | Reordenar por espera (10, 30, 45 y 90 min) | Orden 90, 45, 30, 10 | Correcto |
| 7 | Atender 3 veces con solo 2 pacientes | Atiende a B, luego A, y avisa que ya no hay nadie | Correcto |

También se ejecutó `Main` con 4 pacientes (Juan, Maria, Pedro y Ana). La lista quedó ordenada Pedro, Juan, Ana, Maria. Con un límite de 20 minutos, Pedro y Juan salieron como espera crítica, y el primero en ser atendido fue Pedro.

## 7. Problemas encontrados

- Después de usar `reordenarPorTiempoEspera()`, la lista ya no queda ordenada por prioridad (prueba 6). Si se inserta otro paciente después, puede quedar mal ubicado.
- No se valida nada: se puede poner una prioridad fuera del rango 1 a 5 o minutos negativos.
- Los datos no se guardan; se pierden al cerrar el programa.
