# LAB-3-PDS
## Introducción  
Para la ingeniera Biomédica, la separación de fuentes es clave para mejorar la calidad de las señales que se procesan durante el desarrollo  de la tercera práctica del laboratorio de Procesamiento digital de señales se propone la guía "Problema Fiesta del Coctel" la cual tiene como objetivo la creación de un código en Python el cual busca procesar la voz de los 3 estudiantes del laboratorio hablen en distintas ubicaciones y estas señales sean recibidas por 3 micrófonos distribuidos estratégicamente. El objetivo de la práctica comprender y aplicar conceptos claves de análisis espectral y la separación de señales en un entorno realista para así evaluar el desempeño del sistema en función  de la relación señal ruido SNR y la calidad de la señal recuperada y así proponer mejoras en la metodología utilizada.
## Captura de la Señal para el problema  "fiesta del coctel"
La toma de la señal se llevó a cabo el 24 de marzo de 2025, entre las 3:30 y 4:00 PM, en la Universidad Militar, específicamente en el Edificio de Estudios a Distancia, cuarto piso.

Para el experimento, se utilizaron tres fuentes de sonido identificadas como H, A y F, correspondientes a tres voces diferentes. La captura de la señal se realizó con los siguientes dispositivos:

iPad Pro
iPad
iPhone
Configuración del sistema
Los participantes se distribuyeron en diferentes puntos de la sala, mientras que los dispositivos de grabación se ubicaron de la siguiente manera:

El iPad Pro y el iPad se colocaron sobre la mesa central.
El iPhone se situó en el piso, en un medio escalón.
Para evitar sesgos en la captura de la señal, se intentó orientar los micrófonos de manera que no estuvieran dirigidos hacia el participante más cercano, reduciendo así la influencia directa de una única fuente de sonido sobre cada dispositivo.

La imagen adjunta ilustra la disposición de los elementos y las distancias exactas entre cada fuente de sonido y cada micrófono. Estas medidas permiten evaluar la propagación del sonido y su influencia en la captura de la señal.

Condiciones de Grabación
Ambiente: Espacio cerrado con paredes de vidrio y mobiliario, lo que podría generar reflexiones y reverberación.
Nivel de ruido externo: Relativamente bajo, con posibles interferencias de sonidos ambientales provenientes del exterior.
Frecuencia de muestreo y resolución: (Aquí debes agregar los valores específicos si los tienes).
Tiempo de captura: 18 segundos.
Gracias a esta disposición, se logró registrar una mezcla de voces en cada micrófono, permitiendo la posterior aplicación de técnicas de separación de fuentes en el procesamiento de la señal.
![b6d2aa7b-32d2-4bd8-825b-8dc109608625](https://github.com/user-attachments/assets/204a6149-b674-4dd8-8193-29ccc6ef0ea6)

## 
## Requisistos 
- Python 3.9
- numpy 
- librosa 
- tkinter 
- soundfile 
- matplotlib
-  3 microfonos
-  3 voces

### Contact information
-  est.nikoll.bonilla@unimilitar.edu.co
-  est.hugo.perez@unimilitar.edu.co
-  est.yonatan.franco@unimilitar.edu.co

