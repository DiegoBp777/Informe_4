# Práctica 4: Contador digital de 00 a 99 con displays de 7 segmentos
## Integrantes

- **Jareth Santiago Escamilla Marquez** - [@jarethescamilla](https://github.com/jarethescamilla)
- **Diego Alexander Baron Pacheco** - [@DiegoBp777](https://github.com/DiegoBp777)
- **Fredy Vicente Patiño Garzon** - [@fredyvipatinoga-crypto](https://github.com/fredyvipatinoga-crypto)

---
## Tabla de contenido

1. [Introducción](#1-introducción)
2. [Planteamiento del problema](#2-planteamiento-del-problema)
3. [Objetivos](#3-objetivos)
4. [Materiales y herramientas](#4-materiales-y-herramientas)
5. [Marco teórico](#5-marco-teórico)
6. [Diseño y conexiones del circuito](#6-diseño-y-conexiones-del-circuito)
7. [Funcionamiento del programa](#7-funcionamiento-del-programa)
8. [Algoritmo](#8-algoritmo)
9. [Evidencias](#9-evidencias)
10. [Dificultades y soluciones](#10-dificultades-y-soluciones)
11. [Análisis de resultados](#11-análisis-de-resultados)
12. [Conclusiones](#12-conclusiones)

---

## 1. Introducción

En esta práctica se desarrolló un contador digital de dos dígitos utilizando el microcontrolador PIC18F45K22 y dos displays de siete segmentos. El sistema permite incrementar o disminuir un valor numérico mediante dos pulsadores, mostrando los resultados en un rango de 00 a 99.

Para visualizar los dos dígitos se implementó la técnica de multiplexación, que permite controlar ambos displays utilizando las mismas líneas de segmentos y señales de selección independientes. El programa fue desarrollado en lenguaje C mediante MPLAB X IDE y se verificó su funcionamiento utilizando Proteus y el montaje físico.

Esta práctica permite aplicar conceptos de programación de microcontroladores, configuración de entradas y salidas digitales, lectura de pulsadores, control de displays y multiplexación.

## 2. Planteamiento del problema

Se requiere diseñar e implementar un contador digital capaz de mostrar números entre 00 y 99 utilizando dos displays de siete segmentos controlados por un PIC18F45K22.

El sistema debe contar con dos pulsadores: uno para incrementar el valor y otro para disminuirlo. Cuando el contador llegue a 99 y se presione el pulsador de incremento, debe regresar a 00. De igual manera, si el contador está en 00 y se presiona el pulsador de decremento, debe pasar a 99.

Además, la visualización debe mantenerse estable y legible mediante multiplexación, evitando que ambos displays se activen simultáneamente de forma incorrecta.

## 3. Objetivos

### 3.1. Objetivo general

Implementar un contador digital ascendente y descendente de 00 a 99 mediante un PIC18F45K22 y dos displays de siete segmentos.

### 3.2. Objetivos específicos

- Configurar los puertos del PIC18F45K22 como entradas y salidas digitales.
- Controlar el incremento y decremento del contador mediante dos pulsadores.
- Implementar la conversión del valor numérico en señales para los segmentos.
- Aplicar la técnica de multiplexación para visualizar las decenas y unidades.
- Implementar un mecanismo de antirrebote para mejorar la lectura de los pulsadores.
- Verificar el funcionamiento del sistema en Proteus y en el montaje físico.

## 4. Materiales y herramientas

- Microcontrolador PIC18F45K22.
- Dos displays de siete segmentos modelo 5161AS de cátodo común.
- Dos pulsadores.
- Resistencias de 1 kΩ para limitar la corriente de los segmentos, según el montaje físico.
- Resistencias de 10 kΩ para las entradas de los pulsadores y la polarización de MCLR, según el circuito.
- Protoboard.
- Cables de conexión.
- Fuente de alimentación de 5 V.
- Computador.
- MPLAB X IDE.
- Compilador XC8.
- Proteus para la simulación del circuito.

## 5. Marco teórico

### 5.1. Microcontrolador PIC18F45K22

El PIC18F45K22 es un microcontrolador que permite ejecutar instrucciones programadas y controlar dispositivos electrónicos mediante sus puertos de entrada y salida. En esta práctica se utiliza para leer los pulsadores, realizar las operaciones matemáticas del contador y controlar los displays.

### 5.2. Display de siete segmentos

Un display de siete segmentos está compuesto por siete diodos emisores de luz identificados con las letras A, B, C, D, E, F y G. Mediante diferentes combinaciones de encendido se pueden representar los números del 0 al 9.

En esta práctica se utilizaron displays 5161AS de cátodo común. Para encender un segmento, su terminal correspondiente recibe un nivel lógico alto, mientras que el común del display seleccionado debe conectarse a tierra durante su activación.

Cada segmento debe disponer de una resistencia limitadora de corriente.

### 5.3. Multiplexación

La multiplexación consiste en activar alternativamente cada display a gran velocidad. Primero se muestra el dígito de las decenas y después el de las unidades. Al repetir este proceso continuamente, el ojo humano percibe ambos dígitos encendidos al mismo tiempo.

Esta técnica permite compartir las líneas de los segmentos entre los dos displays, utilizando señales adicionales para seleccionar cuál de ellos se encuentra activo.

### 5.4. Antirrebote de pulsadores

Los pulsadores mecánicos pueden producir pequeñas variaciones eléctricas al presionarse o soltarse. Este fenómeno se conoce como rebote y puede generar varias lecturas de una sola pulsación.

Para reducir este efecto se utiliza un pequeño retardo de estabilización y se espera a que el pulsador sea liberado antes de aceptar una nueva pulsación.

## 6. Diseño y conexiones del circuito

El circuito se construyó utilizando el PIC18F45K22, dos displays de siete segmentos y dos pulsadores.

### 6.1. Conexión de los segmentos

Los segmentos de ambos displays se conectan a las mismas salidas del puerto D del microcontrolador, cada uno con su resistencia limitadora.

| Salida del PIC | Segmento |
|---|---|
| RD0 | A |
| RD1 | B |
| RD2 | C |
| RD3 | D |
| RD4 | E |
| RD5 | F |
| RD6 | G |

El punto decimal (DP) no se utiliza en esta práctica.

### 6.2. Selección de los displays

| Pin del PIC | Función |
|---|---|
| RB0 | Selección del display de las decenas |
| RB1 | Selección del display de las unidades |

Debido a que los displays son de cátodo común y se controlan directamente, la selección se realiza mediante niveles lógicos bajos para activar cada display. Cuando un display no está seleccionado, su señal de control permanece en nivel alto.

Los pines comunes de cada display deben conectarse a su respectiva señal de selección.

### 6.3. Conexión de los pulsadores

| Pin del PIC | Función |
|---|---|
| RA0 | Pulsador de incremento |
| RA1 | Pulsador de decremento |

Las entradas utilizan resistencias pull-up de 10 kΩ conectadas a +5 V. Cada pulsador conecta su entrada correspondiente a GND cuando se presiona.

Por lo tanto, la lectura de la entrada es alta cuando el pulsador está libre y baja cuando se presiona.

### 6.4. Alimentación y reinicio

El PIC debe recibir alimentación de 5 V en sus pines VDD y GND en sus pines VSS. También se debe mantener MCLR en el nivel lógico alto mediante una resistencia de polarización adecuada.

## 7. Funcionamiento del programa

El programa fue desarrollado en lenguaje C utilizando MPLAB X IDE y el compilador XC8.

Su funcionamiento se divide en las siguientes etapas:

1. **Configuración inicial:** se establece la frecuencia del oscilador y se configuran los puertos utilizados como entradas y salidas digitales.
2. **Inicialización del contador:** el valor inicial del contador se establece en 00.
3. **Lectura de los pulsadores:** el programa comprueba si se ha presionado el pulsador de incremento o el de decremento.
4. **Antirrebote:** se aplica un retardo corto y se comprueba nuevamente el estado del pulsador.
5. **Actualización del contador:** el valor aumenta o disminuye según el pulsador utilizado, respetando el rango de 00 a 99.
6. **Separación de dígitos:** las decenas se obtienen mediante la división entera entre 10 y las unidades mediante el residuo de esa división.
7. **Visualización:** se envían los patrones de segmentos correspondientes y se activan alternativamente los displays mediante multiplexación.

La secuencia se repite continuamente para mantener la visualización actualizada y permitir nuevas pulsaciones.

## 8. Algoritmo

### 8.1. Planteamiento del algoritmo

**Entrada:** pulsador de incremento conectado a RA0 y pulsador de decremento conectado a RA1.

**Proceso:** leer los pulsadores, comprobar el antirrebote, actualizar el contador, separar las decenas y unidades, convertir los dígitos a patrones de siete segmentos y realizar la multiplexación.

**Salida:** valor numérico entre 00 y 99 mostrado en los dos displays.

### 8.2. Pseudocódigo

```text
INICIO

Configurar oscilador
Configurar puertos digitales
Configurar pulsadores como entradas
Configurar segmentos y selección como salidas

contador = 0

MIENTRAS el sistema esté funcionando HACER

    Mostrar decenas y unidades mediante multiplexación

    SI se presiona el pulsador de incremento ENTONCES
        Verificar antirrebote

        SI contador = 99 ENTONCES
            contador = 0
        SINO
            contador = contador + 1
        FIN SI

        Esperar liberación del pulsador
    FIN SI

    SI se presiona el pulsador de decremento ENTONCES
        Verificar antirrebote

        SI contador = 0 ENTONCES
            contador = 99
        SINO
            contador = contador - 1
        FIN SI

        Esperar liberación del pulsador
    FIN SI

FIN MIENTRAS

FIN
```

### 8.3. Diagrama general de funcionamiento

```text
        INICIO
           |
           v
 Configurar el PIC
           |
           v
   Inicializar contador
        en 00
           |
           v
 Leer los pulsadores
           |
           v
 ¿Se presionó alguno?
       /       \
     Sí         No
     |           |
     v           |
 Aplicar         |
 antirrebote     |
     |           |
     v           |
 ¿Incrementar    |
 o disminuir?    |
     |           |
     v           |
 Actualizar      |
 el contador     |
     |           |
     v           |
 Separar decenas |
 y unidades      |
     |           |
     v           |
 Multiplexar los |
 dos displays    |
     |           |
     └─────┬─────┘
           |
           v
    Repetir ciclo
```

## 9. Evidencias

En esta sección se presentan las evidencias del desarrollo, simulación y montaje físico de la práctica.

### 9.1. Simulación del contador en Proteus

Se presenta el funcionamiento del contador durante la simulación, verificando la visualización de las decenas y unidades.

![Simulación del contador en Proteus](imagenes/simulacion_proteus.png)

### 9.2. Montaje físico del circuito

Se muestra el montaje realizado en la protoboard, incluyendo el microcontrolador, los displays, los pulsadores y las conexiones de alimentación.

![Montaje físico de la práctica](imagenes/montaje_fisico.png)

## 10. Dificultades y soluciones

Durante el desarrollo de la práctica se identificaron aspectos importantes relacionados con la conexión física, la configuración de los puertos y el control de los displays.

### 10.1. Activación de los displays

Una de las dificultades fue la lógica utilizada para seleccionar los displays. Como se trabajó con displays de cátodo común conectados directamente al microcontrolador, el común del display seleccionado debía ponerse a nivel bajo.

Se corrigió la lógica de selección para que RB0 y RB1 activaran alternativamente los displays, evitando utilizar una condición de encendido contraria al funcionamiento del cátodo común.

### 10.2. Conexión de los pines comunes

Fue necesario revisar las conexiones de los pines comunes de cada display para asegurar que cada uno estuviera asociado a su señal de selección correspondiente. Una conexión incorrecta podía producir una visualización inadecuada o impedir que los dígitos se mostraran correctamente.

### 10.3. Lectura de los pulsadores

Se incorporó una rutina de antirrebote para evitar que una sola pulsación generara múltiples incrementos o decrementos. También se comprobó que el programa esperara la liberación del pulsador antes de aceptar otra pulsación.

### 10.4. Diferencias entre simulación y montaje físico

La simulación en Proteus permitió revisar las conexiones y el comportamiento lógico del programa. Sin embargo, el montaje físico requirió comprobar las conexiones de los displays, la orientación de los pulsadores, la alimentación y las resistencias limitadoras.

Estas verificaciones fueron importantes para lograr un funcionamiento consistente entre la simulación y el circuito real.

## 11. Análisis de resultados

La práctica permitió comprobar la utilidad del microcontrolador PIC18F45K22 para controlar dispositivos de visualización y procesar entradas digitales.

Mediante la implementación del contador se aplicaron operaciones aritméticas para separar las decenas y unidades, así como una tabla de patrones para representar los números del 0 al 9 en los displays.

La multiplexación permitió compartir las líneas de los segmentos y seleccionar cada display de manera alternada. Para conseguir una visualización estable fue necesario mantener el proceso de selección en ejecución continua y revisar la lógica de activación correspondiente a los displays de cátodo común.

El uso de dos pulsadores permitió implementar las operaciones de incremento y decremento, incluyendo el comportamiento circular en los límites de 00 y 99. El antirrebote contribuyó a que cada pulsación se interpretara de forma más controlada.

Las pruebas en Proteus y en el montaje físico ayudaron a identificar y corregir errores de conexión y de lógica de activación. En conjunto, la práctica permitió relacionar el diseño del circuito, la programación y las pruebas experimentales.

## 12. Conclusiones

- Se implementó un contador digital de dos dígitos utilizando el PIC18F45K22 y dos displays de siete segmentos, aplicando los conceptos de programación y control de entradas y salidas digitales.
- Se desarrollaron las funciones de incremento y decremento mediante dos pulsadores, incluyendo el comportamiento circular entre 00 y 99.
- Se aplicó la multiplexación para visualizar las decenas y unidades utilizando líneas de segmentos compartidas, lo que permitió comprender la importancia de seleccionar correctamente cada display.
- Se identificó que la lógica de activación debe corresponder al tipo de display utilizado. En los displays de cátodo común conectados directamente, el terminal común debe llevarse a nivel bajo para habilitar el dígito.
- Las pruebas de simulación y montaje físico permitieron reconocer la importancia de revisar la polarización de las entradas, las conexiones de los pines comunes y las resistencias limitadoras de corriente.
- Finalmente, se cumplieron los objetivos de la práctica al integrar el hardware y el software para construir un contador ascendente y descendente, fortaleciendo los conocimientos sobre microcontroladores, visualización digital y depuración de circuitos electrónicos.
