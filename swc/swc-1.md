# SOFTWARE CRITICO TEMA 1
# Introducción a los Sistemas Críticos
## Sistemas Criticos:
- business critical
- mission critical
- safety critical
    
**Análisis de la Seguridad ≠ Análisis de los Fallos**  
**La seguridad es una propiedad emergente** -> El todo es más que la suma de sus partes, es una característica que se hace evidente solo a un nivel de organización superior.  
propiedades del sistema considerado como un todo, en lugar de como una colección de partes  
Propiedades emergentes:  
- Rendimiento
- Fiabilidad
- Seguridad (safety)
- Seguridad (security)
- Usabilidad
- Mantenibilidad
## Modelos Causales
los accidentes son el resultado de una cadena (o árbol) de eventos de fallo y errores humanos  
- Las relaciones causales son directas y lineales
- La selección de los eventos y causa raiz es subjetivo
### Cadenas de Eventos Causales  
Se consideran: fallos de componentes, errores humanos o eventos relacionados con la energía  
**Objetivos Análisis Causal:**  
-  Asignar responsabilidades
-  Entender por qué ocurrió para evitarlo en el futuro
## Safety I vs Safety II
### Safety I:
se supone que Los sistemas pueden descomponerse y
Los componentes del sistema funcionan de manera bimodal (correcta o incorrecta)
- permite buscar causas y soluciones pero no se ajusta a la realidad  
  
## SOTIF: Safety of the Intended Functionality
no presenta riesgos innecesarios causados por dos cosas:
- Limitaciones o fallos en la función para la que fue diseñado (por ejemplo, que no funcione tan bien como debería).
- Mal uso previsible, es decir, cuando las personas lo usan de una manera que se podía anticipar aunque no sea la correcta.
## Terminologia
 - El iceberg es el **peligro**
 - El **riesgo** lo constituye el que barco pueda colisionar con él
 - Una medida de **mitigación** puede ser pintarlo de amarillo fosforescente
 - El **riesgo residual**, es que nos encontremos con el iceberg por la noche o 
con niebla  
  
Un sub-sistema software puede fallar básicamente de dos formas:
- Puedeno dar una respuesta a tiempo (fallo de disponibilidad)
- Puede dar una respuesta incorrecta (fallo de fiabilidad)  
  
Incrementar la disponibilidad reduce la fiabilidad y viceversa
- Un sistema es confiable/predecible si está disponible y es fiable

## Seguridad funcional
Un sistema puede mejorar su seguridad de dos formas:
- sistemas pasivos de seguridad que evitan que el sistema pueda producir un daño por su diseño `(seguridad pasiva)`
-  Si para que el sistema sea seguro es necesario que uno o varios componentes activos funcionen `seguridad funcional`   
```
We want to reduce the number of faults inserted 
into the system, to detect and remove faults before 
they become errors, to detect errors and prevent 
them from becoming failures and, if a failure does 
occur, to handle it in the safest way possible
```
`FAULT -> ERROR -> FAILURE`

## Recuperación de errores (hacia atrás y hacia adelante)
###  Recuperación hacia atrás:
-  La mayor desventaja: hay que almacenar los estados de vuelta atrás
###  Recuperación hacia adelante:
-  El sistema se conduce a un estado seguro independiente del estado anterior y de la 
entrada  
## Terminología. Sistemas Accidentales      

Un sistema accidental es aquel que no ha sido especificamente diseñado y probado
para ser incorporado en otro sistema que puede tener requisitos críticos de 
seguridad ` software empotrado`  

---
- Hardware: Número suficientemente pequeño de estados para poder ser cubiertos
 todos por técnicas de testing exahustivo
- Softwareen cualquierotro caso

---

- **Bohrbug**: error bien definido, determinista, con propiedades que no cambian 
cuando se añade código para depuración
- **Heisenbug**: error ocasional que aparece y desaparece, que puede cambiar en 
depuración. Suele ser causado por problemas de tiempo, concurrencia,… 
Típicamente el error sería reportado como:

## Estándares de Seguridad y Certificación
**International Electrotechnical Commission (IEC) y la International Organization for Standards (ISO)**
- El uso de diferentes `técnicas` (prevención, tolerancia, análisis y eliminación) en las diferentes 
etapas del ciclo de vida del software
- Que las mismas actividades, técnicas y métodos se utilicen para el `software configurable` y sus 
datos. 
- Diferentes niveles de `independencia del personal`
- Exigencias respecto a la confianza respecto a la `reutilización`
- Exigencias respecto a las `herramientas` utilizadas en el desarrollo

## Estándares de Seguridad Funcional
## `IEC 61508`
Functional safety of electrical/electronic/programmable electronic safety-related 
systems
-  Muchos (aunque no todos) estandares se basan en `IEC 61508`  
    
**SIL: Safety Integrity Level**  
-  IEC 61508 define un conjunto de niveles de integridad (SILs) que se basan 
puramente en la `probabilidad de fallo`, ligando los conceptos de fallos y seguridad (modelo causal) 
    - Un sistema que se usa una vez al año o menos:  
    SIL1 = 0.1  
    SIL4 = 0.0001
    - Un sistema que se usa mas de una vez al año:  
    SIL1 = 10-5  
    SIL4 = 10-8  
  
## `ISO 26262` Especialización de IEC 61508 para coches  
La parte 6, como el estándar anterior incluye tablas de técnicas recomendadas y 
altamente recomendadas para cada `ASIL` (Automotive Satety Integrity Level)
**Los ASIL no están basados en probabilidad de fallo:**  
tipo de heridas,  probabilidad de ocurrir, Cuántos conductores podrían controlar el evento.  

### SEooC: Safety Element out of Context
- Un SEooCes un elemento que va a ser usado en un sistema del automóvil, pero que 
no ha sido específicamente diseñado para ese sistema  

### IEC 62304
- Se centra más en el ciclo de vida de los procesos de desarrollo software para dispositivos 
médicos (dependiendo del posible riesgo al paciente)