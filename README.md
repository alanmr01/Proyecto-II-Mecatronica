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

Como sensor de temperatura, se utilizó un sensor DS18B20.
<div>
<p style = 'text-align:center;'>
<img src="HTB12RHrLmzqK1RjSZFpq6ykSXXaU.jpg" width="400px">
</p>
</div>

Como placa de desarrollo, se utilizó un Arduino UNO.

<div>
<p style = 'text-align:center;'>
<img src="placa-arduino-uno.jpg" width="400px">
</p>
</div>

Para controlar el sensor de temperatura, se utilizó un potenciometro de 5K Ohm.

<div>
<p style = 'text-align:center;'>
<img src="potenciometro.jpg" width="400px">
</p>
</div>

Para la alimentación eléctrica del calefactor y el ventilador, se utiliza una fuente de poder de 60 W de potencia, entregando corriente continua a 12 V.

<div>
<p style = 'text-align:center;'>
<img src="product_template_11304.png" width="400px">
</p>
</div>


Para las conexiones electrónicas, se utilizaron cables Dupont.

<div>
<p style = 'text-align:center;'>
<img src="DMH20-10.png" width="400px">
</p>
</div>

Para las conexiones eléctricas (de mayor potencia electrica), se utilizó cable electrico domiciliario de sección 1.5mm2.

Para asegurar las conexiones mediante soldadura, se utilizaron tubos termoretráctiles en el caso del sensor de temperatura.

### Estructura 

Se utiliza una estructura impresa en PLA para simular la calefacción de una casa. Los archivos en .stl se encuentran en este repositorio.

<div>
<p style = 'text-align:center;'>
<img src="Casa 1.png" width="400px">
</p>
</div>


<div>
<p style = 'text-align:center;'>
<img src="Casa2.png" width="400px">
</p>
</div>

### Procedimiento

Dado que los extremos del cable del sensor de temperatura no tienen conexiones Dupont para conectar directamente a Arduino, estos se deben soldar a cables macho-macho.

<div>
<p style = 'text-align:center;'>
<img src="Soldadura.jpg" width="400px">
</p>
</div>

Utilizando tubos termoretráctiles, se aseguran las soldaduras aislando eléctricamente los cables.

<div>
<p style = 'text-align:center;'>
<img src="TubosTermo.jpg" width="400px">
</p>
</div>

La conexión a la fuente de poder se hace con un cable de instalación domiciliaria.

<div>
<p style = 'text-align:center;'>
<img src="ConexionFuente.jpg" width="400px">
</p>
</div>

Se conectan los modulos relé a la fuente de poder y al dispositivo a controlar.

<div>
<p style = 'text-align:center;'>
<img src="ConexionCalef.jpg" width="400px">
</p>
</div>

Se conecta el sensor de temperatura al potenciometro mediante una protoboard.

<div>
<p style = 'text-align:center;'>
<img src="ConexionPotenciometro.jpg" width="400px">
</p>
</div>

Las conexiones deben quedar de la siguiente forma.

<div>
<p style = 'text-align:center;'>
<img src="ConexionVertical.jpg" width="400px">
</p>
</div>




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
