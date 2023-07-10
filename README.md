# Proyecto-II-Mecatronica






## Fabricación
  
### Materiales

Para la fabricación de


<div>
<p style = 'text-align:center;'>
<img src="Bloque" width="400px">
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
