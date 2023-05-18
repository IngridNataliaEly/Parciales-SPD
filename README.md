# Parcial 1 Sistema de Procesamiento de Datos
Se nos pide armar un modelo de montacarga funcional como maqueta para un hospital. El
objetivo es que implementes un sistema que pueda recibir ordenes de subir, bajar o pausar
desde diferentes pisos y muestre el estado actual del montacargas en el display 7 segmentos.
## Alumna 
Ingrid Natalia Ely

# PARCIAL : Montacarga funcional
![Sin título](https://github.com/IngridNataliaEly/Parciales-SPD/assets/108601149/ae37e359-3eb5-41ca-b689-c1ef5808c162)

## Descripción
El proyecto consiste en un simulador de montacargas controlado por un Arduino. El montacargas puede subir y bajar hasta nueve pisos y se detiene / reanuda cuando se presiona el botón de detener. Utiliza un display de siete segmentos para mostrar el número de piso actual y LEDs para indicar el estado del montacargas.



## Explicación de los componentes:

- Arduino: Es la plataforma de desarrollo utilizada en el proyecto. Se encarga de recibir las señales de los botones, controlar el display de siete segmentos y encender los LEDs.

- Botones (BUTTON_UP, BUTTON_DOWN, BUTTON_STOP): Son los botones utilizados para subir, bajar y detener el montacargas. Se conectan a los pines digitales del Arduino y se utilizan resistencias pull-up internas para simplificar la conexión.

- Display de siete segmentos: Es un componente que muestra números y caracteres alfanuméricos utilizando siete segmentos individuales. En este proyecto, se utiliza para mostrar el número de piso actual del montacargas. Cada segmento del display se conecta a un pin digital del Arduino.

- LEDs (LED_GREEN, LED_RED): Son LEDs utilizados para indicar el estado del montacargas. El LED verde se enciende cuando el montacargas está en movimiento y el LED rojo se enciende cuando el montacargas está detenido. Cada LED se conecta a un pin digital del Arduino.

## Funcionamiento Integral
El proyecto del montacargas controlado por Arduino permite al usuario subir o bajar el montacargas a diferentes pisos. El sistema utiliza botones para que el usuario pueda indicar la dirección deseada (subir o bajar) y detener el montacargas en el piso actual.

Cuando se presiona el botón de subir (BUTTON_UP), el montacargas se moverá hacia arriba si no ha alcanzado el piso máximo (piso 9). Si el montacargas ya está en el piso máximo, se mostrará un mensaje indicando que solo se puede subir hasta el piso 9.

Cuando se presiona el botón de bajar (BUTTON_DOWN), el montacargas se moverá hacia abajo si no ha alcanzado el piso mínimo (piso 1). Si el montacargas ya está en el piso mínimo, se mostrará un mensaje indicando que solo se puede bajar hasta el piso 1.

El botón de detener (BUTTON_STOP) permite al usuario detener o reanudar el movimiento del montacargas. Cuando se presiona el botón de detener, el montacargas se detendrá y se mostrará el piso actual en el display de siete segmentos. El LED RED se encenderá para indicar que el montacargas está detenido. Al presionar nuevamente el botón de detener, el montacargas se reanudará y el LED RED se apagará.

El LED GREEN indica el estado de movimiento del montacargas. Se enciende cuando el montacargas está en movimiento y se apaga cuando está detenido.

El display de siete segmentos muestra el número del piso actual del montacargas. Cada segmento se enciende o apaga según el número que se debe mostrar.

En la función setup(), se configuran los pines utilizados como entradas o salidas, y se inicializa la comunicación serial para la visualización de mensajes.

En el loop(), se verifica el estado de los botones y se realiza la acción correspondiente (subir, bajar o detener). También se actualiza el estado de los LEDs y se muestra el número del piso actual en el display de siete segmentos.

Las funciones Subir(), Bajar() y Display() se encargan de controlar el movimiento del montacargas y mostrar el número del piso en el display de siete segmentos.

El proyecto utiliza las funciones Uno(), Dos(), Tres(), Cuatro(), Cinco(), Seis(), Siete(), Ocho() y Nueve() para encender los segmentos necesarios en el display de siete segmentos y mostrar los dígitos correspondientes.

Este es el funcionamiento integral del proyecto del montacargas controlado por Arduino.

## Enlace 
[Parcial 1 SPD](https://www.tinkercad.com/things/lnlCAm9TDYs)
