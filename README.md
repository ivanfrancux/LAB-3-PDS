# LAB-3-PDS Problema "Fiesta de Cóctel"
## Introducción  
Para la ingeniera Biomédica, la separación de fuentes es clave para mejorar la calidad de las señales que se procesan durante el desarrollo  de la tercera práctica del laboratorio de Procesamiento digital de señales se propone la guía "Problema Fiesta del Coctel" la cual tiene como objetivo la creación de un código en Python el cual busca procesar la voz de los 3 estudiantes del laboratorio hablen en distintas ubicaciones y estas señales sean recibidas por 3 micrófonos distribuidos estratégicamente. El objetivo de la práctica comprender y aplicar conceptos claves de análisis espectral y la separación de señales en un entorno realista para así evaluar el desempeño del sistema en función  de la relación señal ruido SNR y la calidad de la señal recuperada y así proponer mejoras en la metodología utilizada.

## Captura de la Señal para el problema  "Fiesta del Coctel"
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
Tiempo de captura: 18 segundos.
Gracias a esta disposición, se logró registrar una mezcla de voces en cada micrófono, permitiendo la posterior aplicación de técnicas de separación de fuentes en el procesamiento de la señal.
![b6d2aa7b-32d2-4bd8-825b-8dc109608625](https://github.com/user-attachments/assets/204a6149-b674-4dd8-8193-29ccc6ef0ea6)

## Captura de la Señal 
- Procesamiento de los archivos de audio

Primero se cargan a Python usando 'filedialog.askopenfilename()' para que asi quien maneje el programa pueda seleccionar los archivos de tres voces y un archivo con el ruido ambiente.
   ```python
   archivo_voz1 = cargar_archivo('Voz 1')
   archivo_voz2 = cargar_archivo('Voz 2')
   archivo_voz3 = cargar_archivo('Voz 3')
   archivo_ruido = cargar_archivo('Ruido Ambiental')
   ```
Una vez caragados estos archivos se utiliza `librosa.load()`para cargar cada archivo y convertirlo en una señal digital con su frecuencia de muestreo (`sr`).  
```python
   señal_voz1, sr1 = librosa.load(archivo_voz1, sr=None)
   señal_voz2, sr2 = librosa.load(archivo_voz2, sr=None)
   señal_voz3, sr3 = librosa.load(archivo_voz3, sr=None)
   señal_ruido, sr_ruido = librosa.load(archivo_ruido, sr=None)
   ```
Posteriormente se recortan todas las señales al mismo número de muestras `min_len`) para alinearlas y así poder comparararlas de la mejor manera, y asi porcesar las sañles correctamente para que tengan la misma duracion. 
   ```python
 min_len = min(len(señal_voz1), len(señal_voz2), len(señal_voz3), len(señal_ruido))
   señal_voz1 = señal_voz1[:min_len]
   señal_voz2 = señal_voz2[:min_len]
   señal_voz3 = señal_voz3[:min_len]
   señal_ruido = señal_ruido[:min_len]
   ```
- Digitalizacion de la señal.
   - Frecuencia de muestreo: La frecuencia de muestreo se obtiene automáticamente cuando se carga el archivo de audio con `librosa.load()`, usando `sr=None` para mantener la frecuencia original del archivo, utilizando `sr_senal`para represenytar la frecuencia de muestreo de la señal de voz
     
   ```python
   señal, sr_senal = librosa.load(audio_senal, sr=None)
   ruido, sr_ruido = librosa.load(audio_ruido, sr=None)
   ```  

   - Niveles de cuantificación:  En 'librosa' se normaliza las señales con valores entre -1 y 1 lo que hace que la cuantificacion este en un punto flotante y no con valores enteros tipocos de PCM  (Pulse Code Modulation, o Modulación por Código de Pulsos).
   -Tiempo de captura: El tiempo de captura se puede calcular dividiendo el número de muestras por la frecuencia de muestreo, siendo la cantidad de muestras de la señal (`len(señal)`) se divide por la frecuencia de muestreo (`sr_senal`) para obtener el tiempo en segundos.  


   ```python
   tiempo_captura = len(señal) / sr_senal
   ```
     -Relación Señal-Ruido (SNR):la cual se calcula comparando la potencia de la señal con la del ruido, tomando  la media de los valores al cuadrado (potencia), y se aplica la fórmula del SNR en decibeles
   ```python
   potencia_senal = np.mean(señal**2)
   potencia_ruido = np.mean(ruido**2)
   snr = 10 * np.log10(potencia_senal / potencia_ruido)
   ```

## Análisis Temporal y Frecuencial 
-Análisis en el Dominio del Tiempo 
Gráficas de la señal en el dominio temporal: Se visualizan con `plt.plot(señal)`. Para permiten observar la amplitud y la forma de onda.


```python
plt.subplot(2, 1, 1)
plt.plot(señal, label="Señal Original")
plt.title("Análisis Temporal de la Señal")
plt.xlabel("Tiempo (muestras)")
plt.ylabel("Amplitud")
plt.legend()
```

	-Análisis en el Dominio de la Frecuencia
	Para Espectros de frecuencia: Se obtiene con `np.fft.fft(señal)` para que la escala sea adecuada en el dominio de la frecuencia utilizamos la escala semilogarítmica `plt.semilogy()` para visualizar mejor los componentes frecuenciales.

```python
frecuencia = np.fft.fftfreq(len(señal), d=1/sr)
espectro = np.abs(np.fft.fft(señal))

plt.subplot(2, 1, 2)
plt.semilogy(frecuencia[:len(señal)//2], espectro[:len(señal)//2])
plt.title("Análisis Frecuencial (Espectro de Frecuencia)")
plt.xlabel("Frecuencia (Hz)")
plt.ylabel("Magnitud (Escala logarítmica)")
plt.show()
```

## Separacion de Fuentes 
- Métodos Utilizados
	- Descripción del algoritmo de separación se usa la resta entre la señal original y el ruido (`señal - ruido`) para obtener una señal limpia.
  
  ```python
  señal_sin_ruido = señal - ruido
  señal_sin_ruido = np.clip(señal_sin_ruido, -1.0, 1.0)  # Evita saturación
  sf.write("salida_sin_ruido.wav", señal_sin_ruido, sr_senal)
  ```
 
 Asumiendo que el ruido capturado es similar al presente en la señal original para permitir la cancelación efectiva.

- Resultados Obtenidos**
	- Evaluación cualitativa y cuantitativa se calcula el SNR antes y después de la eliminación del ruido para evaluar la mejora en la calidad.

  ```python
  snr = 10 * np.log10(np.mean(señal**2) / np.mean(ruido**2))
  ```
	- Discusión sobre la calidad del audio se presentan los archivos generados (`salida_sin_ruido.wav` y `voz_filtrada.wav`), evaluando subjetivamente la mejora en claridad.

   ```python
  messagebox.showinfo("Resultado", f'El archivo filtrado se guardó como: {archivo_filtrado}'
   ```   
## Resultados 
![8e0e4b13-cb1e-410b-a205-a987ad07646b](https://github.com/user-attachments/assets/9dad66e0-2a8a-49c8-b5a6-5e0399aca045)

![ce406a2c-e788-4fd0-b75c-6ede96abfa14](https://github.com/user-attachments/assets/39a2b798-1ff1-4a54-90b6-abc921a29c1c)

## Cocluciones 


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

