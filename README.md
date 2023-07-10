# Proyecto-II-Mecatronica






## Fabricación
  
### Materiales

Para la fabricación del proyecto, se utilizaron los siguientes materiales:

Cartucho calefactor de impresora 3D, de potencia 40 W y alimentación 12V.

<div>
<p style = 'text-align:center;'>
<img src="Cartucho-calefactor-12V-40W.jpg" width="400px">
</p>
</div>

Bloque de aluminio para cartucho calefactor de impresora 3D, para mejorar la disipación de calor y aumentar la velocidad de la calefacción.
<div>
<p style = 'text-align:center;'>
<img src="Bloque.jpg" width="400px">
</p>
</div>

Ventilador de 12V, para expulsar el aire caliente y enfriar el sistema.

<div>
<p style = 'text-align:center;'>
<img src="429-1.jpg" width="400px">
</p>
</div>

Para controlar el encendido y apagado del calefactor y ventilador, se utilizaron 2 módulos relé de 1 canal.

<div>
<p style = 'text-align:center;'>
<img src="Rele.jpg" width="400px">
</p>
</div>


### Procedimiento





### Código y programación del Arduino

El codigo en C++ utilizado fue el siguiente:
  
```C++
#include <OneWire.h>                
#include <DallasTemperature.h>
 
OneWire ourWire(3);                //Se establece el pin 2  como bus OneWire
int pinVent = 8;
int pinCalef = 9;

unsigned long T_inicial;


float TempDeseada = 27.00;
 
DallasTemperature sensors(&ourWire); //Se declara una variable u objeto para nuestro sensor

void setup() {
delay(1000);
Serial.begin(9600);
sensors.begin();   //Se inicia el sensor


pinMode(pinVent, OUTPUT);
pinMode(pinCalef,OUTPUT);
digitalWrite(pinVent,LOW);
digitalWrite(pinCalef, LOW);
}
 
void loop() {
sensors.requestTemperatures();   //Se envía el comando para leer la temperatura
float temp= sensors.getTempCByIndex(0); //Se obtiene la temperatura en ºC

Serial.print("Temperatura= ");
Serial.print(temp);
Serial.println(" C");
delay(100); 

if (TempDeseada<temp && millis()>10000){
  digitalWrite(pinVent,HIGH);
  digitalWrite(pinCalef, LOW);
}


if (TempDeseada>temp && millis()>10000){
  T_inicial = millis();
  while (millis()-T_inicial<10000){
    digitalWrite(pinCalef,HIGH);
    digitalWrite(pinVent,LOW);}
  T_inicial= millis();
  while (millis()-T_inicial<30000){
  digitalWrite(pinCalef,LOW);}
}

if (TempDeseada==temp){
  T_inicial=millis();
  while (millis()-T_inicial < 5000){
      digitalWrite(pinCalef,LOW);
      digitalWrite(pinVent,LOW);}
   }
}
```
