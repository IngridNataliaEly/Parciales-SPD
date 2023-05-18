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

~~~
#define BUTTON_UP 2
#define BUTTON_DOWN 3
#define BUTTON_STOP 4
#define LED_GREEN 5
#define LED_RED 6
#define SEG_A 7
#define SEG_B 8 
#define SEG_C 9 
#define SEG_D 10
#define SEG_E 11
#define SEG_F 12
#define SEG_G 13
#define TAM_INI 9

int estado;
//int estadoLed = LOW;
int estadoAnterior = LOW;


const int segmentos[] = {5,6,7, 8, 9, 10, 11, 12, 13};
bool seMueve = false;
bool parado = false;
int pisos = 0;

//variables para boton parar
int estadoBoton;
int estadoAnteriorBoton = 1;
int estadoLed = 0;

void setup()
{
 for(int i= 0;i<TAM_INI;i++)
  {
    pinMode(segmentos[i], OUTPUT);  
  }
  pinMode(BUTTON_UP, INPUT_PULLUP);//explico porque
  pinMode(BUTTON_DOWN, INPUT_PULLUP);
  pinMode(BUTTON_STOP, INPUT_PULLUP);

// Inicializar el monitor serial
  Serial.begin(9600);
  Serial.println("Montacargas listo para operar");
}
void loop()
{  
  
  if (digitalRead(BUTTON_UP) == LOW) 
  {
    if(pisos >= 9)
    {
    	pisos = 9;
      	Serial.println("Solo se puede subir hasta el piso 9");
    }
    else
    {
    	Subir();
    }
  } 
  else if (digitalRead(BUTTON_DOWN) == LOW) 
  {
    if(pisos <=1 )
    {
    	pisos = 1;
      	Serial.println("Solo se puede bajar hasta el piso 1");
    }
    else
    {
    	 Bajar();
    }   
  } 
  estado = digitalRead(BUTTON_STOP);
  if(estado != estadoAnterior)
  {
    if (estado == LOW)
    {
      estadoLed = !estadoLed;
      digitalWrite(LED_RED,estadoLed);
      if(estadoLed == HIGH)
      {
      	parado =  true;
        Display(pisos);
        Serial.print("Montacargas detenido en el piso: ");
        Serial.println(pisos); 
      }
      else
      {
      	parado =  false;
        Display(pisos);
        Serial.print("Montacargas reanudado: ");
        Serial.println(pisos); 
      }
    }
    estadoAnterior = estado;
  }
  delay(50);
  
}  
void Subir()
{
  if(!parado )
  { 
   seMueve = true;  
   digitalWrite(LED_GREEN, HIGH);
   pisos++;
   Serial.print("Montacargas se esta moviendo Piso: ");
   Serial.println(pisos);
   Display(pisos);
   digitalWrite(LED_GREEN, LOW);
  }
  else
  {
    pisos = 0;
  }
}  
void Bajar()
{
  if(!parado && pisos >= 0 && pisos <= 9)
  { 
 	seMueve = true;    
  	digitalWrite(LED_GREEN, HIGH); 
 	pisos--;
  	Serial.print("Montacargas se esta moviendo Piso: ");
  	Serial.println(pisos);
  	Display(pisos);  
  	digitalWrite(LED_GREEN, LOW);
  }
  else
  {
    pisos = 0;
  }
} 
void Display(int pisos)
{
  switch(pisos)
  {
  	case 1:
      Uno(SEG_B,SEG_C,3000); 
    break;
    case 2:
      Dos(SEG_A,SEG_B,SEG_G,SEG_E,SEG_D,3000);
    break;
    case 3:
     Tres(SEG_A,SEG_B,SEG_G,SEG_C,SEG_D,3000);
    break;
    case 4:
      Cuatro(SEG_F,SEG_G,SEG_B,SEG_C,3000);
    break;
    case 5:
      Cinco(SEG_A,SEG_F,SEG_G,SEG_C,SEG_D,3000);	
    break;
    case 6:
      Seis(SEG_A,SEG_F,SEG_G,SEG_E,SEG_D,SEG_C,3000);
    break;
    case 7:
      Siete(SEG_A,SEG_B,SEG_C,3000);
    break;
    case 8:
      Ocho(SEG_A,SEG_B,SEG_C,SEG_D,SEG_E,SEG_F,SEG_G,3000);
    break;
    case 9:
      Nueve(SEG_F,SEG_A,SEG_B,SEG_G,SEG_C,3000);
    break;
}

}
void Cero(int pin1,int pin2,int pin3,int pin4,int pin5,int pin6, int tiempo){
  digitalWrite(pin1, HIGH);
  digitalWrite(pin2, HIGH);
  digitalWrite(pin3, HIGH);
  digitalWrite(pin4, HIGH);
  digitalWrite(pin5, HIGH);
  digitalWrite(pin6, HIGH);
  delay(tiempo); 
  digitalWrite(pin1, LOW);
  digitalWrite(pin2, LOW);
  digitalWrite(pin3, LOW);
  digitalWrite(pin4, LOW);
  digitalWrite(pin5, LOW);
  digitalWrite(pin6, LOW);
  delay(0); 
}
void Uno(int pin1,int pin2,int tiempo)
{
  digitalWrite(pin1, HIGH);
  digitalWrite(pin2, HIGH);
  delay(tiempo); 
  digitalWrite(pin1, LOW);
  digitalWrite(pin2, LOW);
  delay(0); 
}  
 void Dos(int pin1,int pin2,int pin3,int pin4,int pin5, int tiempo)
{
  digitalWrite(pin1, HIGH);
  digitalWrite(pin2, HIGH);
  digitalWrite(pin3, HIGH);
  digitalWrite(pin4, HIGH);
  digitalWrite(pin5, HIGH);
  delay(tiempo); 
  digitalWrite(pin1, LOW);
  digitalWrite(pin2, LOW);
  digitalWrite(pin3, LOW);
  digitalWrite(pin4, LOW);
  digitalWrite(pin5, LOW);
  delay(0); 
} 
void Tres(int pin1,int pin2,int pin3,int pin4,int pin5, int tiempo)
{
  digitalWrite(pin1, HIGH);
  digitalWrite(pin2, HIGH);
  digitalWrite(pin3, HIGH);
  digitalWrite(pin4, HIGH);
  digitalWrite(pin5, HIGH);
  delay(tiempo); 
  digitalWrite(pin1, LOW);
  digitalWrite(pin2, LOW);
  digitalWrite(pin3, LOW);
  digitalWrite(pin4, LOW);
  digitalWrite(pin5, LOW);
  delay(0); 
}
void Cuatro(int pin1,int pin2,int pin3,int pin4, int tiempo)
{
  digitalWrite(pin1, HIGH);
  digitalWrite(pin2, HIGH);
  digitalWrite(pin3, HIGH);
  digitalWrite(pin4, HIGH);
  delay(tiempo); 
  digitalWrite(pin1, LOW);
  digitalWrite(pin2, LOW);
  digitalWrite(pin3, LOW);
  digitalWrite(pin4, LOW);
  delay(0); 
}
void Cinco(int pin1,int pin2,int pin3,int pin4,int pin5, int tiempo)
{
  digitalWrite(pin1, HIGH);
  digitalWrite(pin2, HIGH);
  digitalWrite(pin3, HIGH);
  digitalWrite(pin4, HIGH);
  digitalWrite(pin5, HIGH);
  delay(tiempo); 
  digitalWrite(pin1, LOW);
  digitalWrite(pin2, LOW);
  digitalWrite(pin3, LOW);
  digitalWrite(pin4, LOW);
  digitalWrite(pin5, LOW);
  delay(0); 
} 
void Seis(int pin1,int pin2,int pin3,int pin4,int pin5,int pin6, int tiempo)
{
  digitalWrite(pin1, HIGH);
  digitalWrite(pin2, HIGH);
  digitalWrite(pin3, HIGH);
  digitalWrite(pin4, HIGH);
  digitalWrite(pin5, HIGH);
  digitalWrite(pin6, HIGH);
  delay(tiempo); 
  digitalWrite(pin1, LOW);
  digitalWrite(pin2, LOW);
  digitalWrite(pin3, LOW);
  digitalWrite(pin4, LOW);
  digitalWrite(pin5, LOW);
  digitalWrite(pin6, LOW);
  delay(0); 
}
void Siete(int pin1,int pin2,int pin3, int tiempo)
{
  digitalWrite(pin1, HIGH);
  digitalWrite(pin2, HIGH);
  digitalWrite(pin3, HIGH);
  delay(tiempo); 
  digitalWrite(pin1, LOW);
  digitalWrite(pin2, LOW);
  digitalWrite(pin3, LOW);
  delay(0); 
}
void Ocho(int pin1,int pin2,int pin3,int pin4,int pin5,int pin6,int pin7, int tiempo)
{
  digitalWrite(pin1, HIGH);
  digitalWrite(pin2, HIGH);
  digitalWrite(pin3, HIGH);
  digitalWrite(pin4, HIGH);
  digitalWrite(pin5, HIGH);
  digitalWrite(pin6, HIGH);
  digitalWrite(pin7, HIGH);
  delay(tiempo); 
  digitalWrite(pin1, LOW);
  digitalWrite(pin2, LOW);
  digitalWrite(pin3, LOW);
  digitalWrite(pin4, LOW);
  digitalWrite(pin5, LOW);
  digitalWrite(pin6, LOW);
  digitalWrite(pin7, LOW);
  delay(tiempo); 
}
void Nueve(int pin1,int pin2,int pin3,int pin4,int pin5, int tiempo)
{
  digitalWrite(pin1, HIGH);
  digitalWrite(pin2, HIGH);
  digitalWrite(pin3, HIGH);
  digitalWrite(pin4, HIGH);
  digitalWrite(pin5, HIGH);
  delay(tiempo); 
  digitalWrite(pin1, LOW);
  digitalWrite(pin2, LOW);
  digitalWrite(pin3, LOW);
  digitalWrite(pin4, LOW);
  digitalWrite(pin5, LOW);
  delay(tiempo); 
} 

~~~





## Funcionamiento Integral
El proyecto del montacargas controlado por Arduino permite al usuario subir o bajar el montacargas a diferentes pisos. El sistema utiliza botones para que el usuario pueda indicar la dirección deseada (subir o bajar) y detener el montacargas en el piso actual.

***Cuando se presiona el botón de subir (BUTTON_UP), el montacargas se moverá hacia arriba si no ha alcanzado el piso máximo (piso 9). Si el montacargas ya está en el piso máximo, se mostrará un mensaje indicando que solo se puede subir hasta el piso 9.***


![Captura de pantalla (97)](https://github.com/IngridNataliaEly/Parciales-SPD/assets/108601149/b9819bdf-b0ae-44a1-902c-188ca3d7492a)




***Cuando se presiona el botón de bajar (BUTTON_DOWN), el montacargas se moverá hacia abajo si no ha alcanzado el piso mínimo (piso 1). Si el montacargas ya está en el piso mínimo, se mostrará un mensaje indicando que solo se puede bajar hasta el piso 1.***


![Captura de pantalla (98)](https://github.com/IngridNataliaEly/Parciales-SPD/assets/108601149/c498a4ce-a7c7-403c-b6a8-0cf96426dc11)

***El botón de detener (BUTTON_STOP) permite al usuario detener o reanudar el movimiento del montacargas. Cuando se presiona el botón de detener, el montacargas se detendrá y se mostrará el piso actual en el display de siete segmentos. El LED RED se encenderá para indicar que el montacargas está detenido. Al presionar nuevamente el botón de detener, el montacargas se reanudará y el LED RED se apagará.***


![Captura de pantalla (100)](https://github.com/IngridNataliaEly/Parciales-SPD/assets/108601149/7108f5f6-39e9-4458-9133-c9647b6b5448)

El LED GREEN indica el estado de movimiento del montacargas. Se enciende cuando el montacargas está en movimiento y se apaga cuando está detenido.

El display de siete segmentos muestra el número del piso actual del montacargas. Cada segmento se enciende o apaga según el número que se debe mostrar.

En la función setup(), se configuran los pines utilizados como entradas o salidas, y se inicializa la comunicación serial para la visualización de mensajes.

En el loop(), se verifica el estado de los botones y se realiza la acción correspondiente (subir, bajar o detener). También se actualiza el estado de los LEDs y se muestra el número del piso actual en el display de siete segmentos.

Las funciones Subir(), Bajar() y Display() se encargan de controlar el movimiento del montacargas y mostrar el número del piso en el display de siete segmentos.

El proyecto utiliza las funciones Uno(), Dos(), Tres(), Cuatro(), Cinco(), Seis(), Siete(), Ocho() y Nueve() para encender los segmentos necesarios en el display de siete segmentos y mostrar los dígitos correspondientes.

Este es el funcionamiento integral del proyecto del montacargas controlado por Arduino.

## Enlace 
[Parcial 1 SPD](https://www.tinkercad.com/things/lnlCAm9TDYs)
