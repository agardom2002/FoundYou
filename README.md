# Fire Detector
***Proyecto realizado por [Alejandro Fernández Romero](https://github.com/AlexFdezRomero) y [Andrés García Domínguez](https://github.com/agardom2002).***

## Índice
✔️[1. Justificación y descripción del proyecto.](#id1)

[2. Obtención de datos.](#id2)

[3. Limpieza de datos.](#id3) 

[4. Exploración y visualización.](#id4)

✔️[5. Preparación de los datos para los algoritmos de Machine Learning.](#id5)

✔️[6. Entrenamiento del modelo y comprobación del rendimiento.](#id6)

[7. NPL](#id7)

[8. Aplicación web.](#id8)

[9. Conclusiones.](#id9)

## 1. Justificación y descripción del proyecto.<a name="id1"></a>

🔥 ¡Atención a todos los amantes de la tecnología y la seguridad! 🔥 ¿Estás buscando una manera innovadora de proteger nuestros preciosos recursos naturales? ¡Entonces, este es el proyecto que estabas esperando!

🚒 Presentamos nuestro increíble Trabajo de Fin de Máster en Inteligencia Artificial y Big Data: ¡La solución definitiva para la detección de incendios en áreas naturales!

🌳 En un mundo donde los incendios forestales representan una seria amenaza para el medio ambiente y la seguridad pública, ¡nuestro proyecto brinda una solución inteligente y efectiva!

💡 Con la combinación de algoritmos de Inteligencia Artificial y análisis de Big Data, nuestro sistema puede detectar la presencia de fuego en áreas naturales de manera precisa y oportuna. ¡No más preocupaciones por incendios no detectados!

✅ ¿Por qué elegir nuestra solución?

- Eficiencia: Nuestro sistema utiliza algoritmos avanzados para analizar imágenes y datos en tiempo real, lo que permite una detección temprana y rápida de incendios.
- Precisión: Gracias a la inteligencia artificial, nuestro sistema reduce al mínimo los falsos positivos, garantizando una detección confiable.
- Escalabilidad: Diseñado para adaptarse a diferentes entornos y escalas, desde pequeñas áreas forestales hasta vastas extensiones de terreno.
- Facilidad de uso: Interfaz intuitiva y amigable que permite a los usuarios monitorear y gestionar el sistema con facilidad.
- Impacto ambiental: Al detectar incendios de manera temprana, ayudamos a prevenir la propagación y minimizamos el impacto ambiental y económico de los incendios forestales.
📈 ¡Únete a la revolución de la seguridad ambiental y haz la diferencia hoy mismo! ¡No esperes más para proteger nuestro planeta con nuestra innovadora solución de detección de incendios!

🔥 ¡No dejes que el fuego arruine nuestro futuro! 🔥

Este proyecto se basa en un modelo de reconocimiento y segmentación de imágenes en tiempo real entrenado para detectar si hay fuego en la imagen, utiliza un algoritmo llamado YOLO que se centra en este tipo de modelos
haciendo que sea mucho más veloz y preciso (Enlace a su [GitHub](https://github.com/ultralytics/ultralytics)).

<img src="https://drive.google.com/uc?id=1CURd9SyhdrQgs0kD-ZFQW8m44clrS6bJ" height="600px">

***Imagen obtenida de la predicción del modelo entrenado.***

## 2. Obtención de datos.<a name="id2"></a>

Al utilizar el algoritmo YOLOv8, necesitamos tanto las imágenes para entrenar el modelo cómo la segmentación de estas imágenes para indicar cuál es el target, en este caso el fuego.
Para ello, vamos a utilizar un dataset obtenido desde la página [Roboflow](https://universe.roboflow.com/aj-garcia-736tc/fire-dataset-for-yolov8).

<img src="https://drive.google.com/uc?id=1CCq5_j7wUpI4lIWwtwRNHIPj7MLmnPVK" height="150px">

## 3. Limpieza de datos. <a name="id3"></a>
## 4. Exploración y visualización.<a name="id4"></a>
## 5. Preparación de los datos para los algoritmos de Machine Learning.<a name="id5"></a>

Si queremos segmentar imágenes manualmente para añadirlas al dataset podemos instalar en nuestro equipo **labelme**. En este caso lo hemos instalado y utilizado con Visual Studio Code.
```python
pip install labelme
```

Una vez instalado, se puede iniciar escribiendo **labelme** en la terminal. 
Indicamos la carpeta donde se encuentran las imágenes a segmentar. El sigiente paso es mediante el ratón, indicar la zona del objetivo (target) y etiquetarlo. 
Con cada imagen, se genera un archivo .json que indica las coordenadas de la segmentación del target en la imagen.

<img src="https://drive.google.com/uc?id=1j5LO06FLwN3qexNPkma1XLXLHwTlQCou" height="300px">

Cuando se hayan segmentado y etiquetado todas las imágenes tenemos que instalar **labelme2yolo** para transformar los datos para el algoritmo.

```python
pip install labelme2yolo
```
Tras instalarlo debemos ejecutar el siguiente comando para preparar los datos:

```python
labelme2yolo --json_dir "Ruta de la carpeta con las imágenes"
```

Se puede observar que al ejecutar la transformación se genera una carpeta que prepara el dataset para el entrenamiento, separa por un lado las imágenes de los archivos .json asociados. Además,
crea un archivo **dataset.yaml** que será el que utilizaremos para realizar el entrenamiento.

<img src="https://drive.google.com/uc?id=1qTqUW3hMEv5jayhPxI3plWtiEEChNsSe" height="250px">

## 6. Entrenamiento del modelo y comprobación del rendimiento.<a name="id6"></a>

Enlace al entrenamiento en un documento de [Google Colab](https://colab.research.google.com/drive/1mmFQI4K9Ic9whAI8TFCMuOtnLl3uUM4S?usp=sharing).

### **Entrenamiento del modelo YOLOv8**

Instalación de los paquetes necesarios:

* **ultralytics:** Para obtener y entrenar el modelo
* **roboflow:** Para descargar el dataset de imágenes para el entrenamiento

```python
!pip install ultralytics==8.0.196
!pip install roboflow
```
Importamos las librerías necesarias tanto para descargar el dataset como para entrenar el modelo

```python
import ultralytics
from roboflow import Roboflow
from ultralytics import YOLO
```
Descargamos el dataset de imágenes. Para ello usamos la API proporcionada de Roboflow

```python
rf = Roboflow(api_key="MAiCeSuy58yjlg2ma4QK")
project = rf.workspace("-jwzpw").project("continuous_fire")
dataset = project.version(6).download("yolov8")
```
Comando para realizar el entrenamiento del modelo

Los diferentes parámetros que se usan son :

- **task**: Sirve para indicar la tarea a realizar.
  - detect: Consiste en detectar objetos en una imagen o video, dibujando cuadros a su alrededor calificándolos según sus características. Puede detectar varios objetos a la vez.
  - segment: Segmenta una imagen en diferentes regiones basándose en el contenido de la imagen. A cada region se la asigna una etiqueta. En diferencia con *detect*, no es un cuadro, es la silueta del objeto.
  - classify: Clasifica una imagen en diferentes categorías basándose en su contenido.
  - pose: Detecta los puntos clave de un objeto en un fotograma y se utilizan
  para seguir el movimiento o estimar la pose.

- **mode**:
  - train: Afinar el modelo en un conjunto de datos personalizados o precargados. El proceso consiste en optimizar los parámetros para una mayor
  precisión.
  - val: Se utiliza para un modelo una vez entrenado. Evalúa su precisión y
  rendimiento, permitiendo ajustar los hiperparámetros para mejorar su rendimiento.
  - predict: Se carga el modelo entrenado y se le proporcionan nuevas imágenes o videos para ver su funcionamiento.
  - export: Permite exportar el modelo a un formato que pueda utilizarse para su
  despliegue.
  - track: Modo seguimiento. Con el modelo entrenado, se le proporciona un flujo de vídeo en directo para seguir objetos en tiempo real.
  - benchmark: Su utiliza para perfilar la velocidad y precisión de varios formatos de exportación. Con información como; el tamaño del formato exportado, las diferentes métricas y el tiempo de inferencia por imagen, podemos elegir el formato más óptimo.

- **model**: Modelo a utilizar, en este caso *yolov8s.pt*.
Para yolov8 hay diferentes variantes; **n** (nano), **s** (small), **m** (medium), **l** (large) y **x** (extra large).

- **data**: Ruta donde se encuentra el archivo **.yaml** que indica los diferentes directorios de entrenamiento y validación.

- **epochs**: Establecer el número de iteraciones de los datos de entrenamiento.

- **imgsz**: Especificar el tamaño de las imágenes.

- **plots**: Indicar que se generen gráficas para evaluar el rendimiento del entrenamiento.
```python
!yolo task=detect mode=train model=yolov8s.pt data=/content/continuous_fire-6/data.yaml epochs=80 imgsz=640 plots=True
```
Forma para descargar la carpeta runs de forma local en nuestro equipo

```python
from google.colab import files
files.download('/content/runs')
```
Montar nuestro Google Drive
```python
from google.colab import drive
drive.mount('/content/drive')
```
Copiamos la carpeta runs a un directorio de nuestro Google Drive
```python
!cp -r /content/runs /content/drive/MyDrive/runs
```

Tras terminar el entrenamiento vemos un resumen de las iteraciones que ha realizado:

| **epoch** | **train/box_loss** | **train/cls_loss** | **train/dfl_loss** | **metrics/precision(B)** | **metrics/recall(B)** | **metrics/mAP50(B)** | **metrics/mAP50-95(B)** | **val/box_loss** | **val/cls_loss** | **val/dfl_loss** | **lr/pg0** | **lr/pg1** | **lr/pg2** |
|-----------|--------------------|--------------------|--------------------|--------------------------|-----------------------|----------------------|-------------------------|------------------|------------------|------------------|------------|------------|------------|
| 1         | 1.5482             | 2.4676             | 1.52               | 0.2994                   | 0.45368               | 0.31052              | 0.15748                 | 1.6036           | 3.9442           | 1.6668           | 0.00065608 | 0.00065608 | 0.00065608 |
| 2         | 1.5498             | 1.7261             | 1.5374             | 0.15422                  | 0.24947               | 0.09754              | 0.03113                 | 2.5966           | 10.198           | 3.1666           | 0.0013064  | 0.0013064  | 0.0013064  |
| 3         | 1.6696             | 1.7763             | 1.6429             | 0.17021                  | 0.43368               | 0.1711               | 0.05997                 | 1.9944           | 12.878           | 2.7897           | 0.0019402  | 0.0019402  | 0.0019402  |
| 4         | 1.6521             | 1.678              | 1.6184             | 0.02385                  | 0.07895               | 0.00919              | 0.0037                  | 2.7676           | 16.016           | 2.7448           | 0.0019258  | 0.0019258  | 0.0019258  |
| 5         | 1.5977             | 1.5831             | 1.5863             | 0.59603                  | 0.60211               | 0.56022              | 0.30581                 | 1.5828           | 2.1703           | 1.6896           | 0.0019258  | 0.0019258  | 0.0019258  |
| ...        | ...             | ...            | ...              | ...                   | ...               | ...              | ...                  | ...           | ...          | ...            | ...  | ...  | ...  |
| 75        | 0.75444            | 0.47607            | 1.0348             | 0.86927                  | 0.78105               | 0.87979              | 0.64218                 | 0.97949          | 0.8365           | 1.2372           | 0.00019325 | 0.00019325 | 0.00019325 |
| 76        | 0.73771            | 0.46127            | 1.0283             | 0.86209                  | 0.79789               | 0.88628              | 0.64989                 | 0.97961          | 0.83844          | 1.2291           | 0.0001685  | 0.0001685  | 0.0001685  |
| 77        | 0.72293            | 0.44233            | 1.0044             | 0.87006                  | 0.79368               | 0.88495              | 0.64534                 | 0.97614          | 0.82528          | 1.2272           | 0.00014375 | 0.00014375 | 0.00014375 |
| 78        | 0.70837            | 0.43255            | 1.0045             | 0.87084                  | 0.80947               | 0.88686              | 0.65076                 | 0.97163          | 0.80264          | 1.221            | 0.000119   | 0.000119   | 0.000119   |
| 79        | 0.70564            | 0.4307             | 1.0002             | 0.8831                   | 0.80211               | 0.89361              | 0.65726                 | 0.96027          | 0.77231          | 1.208            | 9.425e-05  | 9.425e-05  | 9.425e-05  |
| 80        | 0.69584            | 0.42559            | 0.99257            | 0.87153                  | 0.81407               | 0.89305              | 0.65215                 | 0.97947          | 0.79239          | 1.2251           | 6.95e-05   | 6.95e-05   | 6.95e-05   |

Observamos la matriz de confusión y nos aseguramos de que no detecta el fondo como fuego y viceversa.

<img src="https://drive.google.com/uc?id=11DGOclWTlVWHeLv-AB8nnOcpaDvlsyO4" height="250px">

### Comprobación en local con cámara

Para comprobar el funcionamiento del modelo, primero conectameros el modelo a una cámara de manera local.

Verificamos que tenemos ultralytics instalado.
```python
!pip install ultralytics
```
Instalamos la librería OpenCV para poder trabajar con la cámara.
```python
!pip install opencv-python==4.6.0.66
```

Importamos las librerias necesarias.
```python
import ultralytics
import cv2
from ultralytics import YOLO
from IPython.display import Image
```
Ejecutamos el código para poner en marcha la cámara con el modelo integrado:

```python
# Leer nuestro modelo, tenemos que especificar la ruta del modelo.
model = YOLO("best.pt")
# Capturar video
cap =  cv2.VideoCapture(1)

# Bucle
while True:
    # Leer fotogramas
    ret, frame = cap.read()

    # Leemos resultados
    resultados = model.predict(frame, imgsz = 640, conf=0.80)

    # Mostramos resultados
    anotaciones = resultados[0].plot()

    # Mostramos nuestros fotogramas
    cv2.imshow("DETECCION Y SEGMENTACION", anotaciones)

    # Cerrar nuestro programa
    if cv2.waitKey(1) == 27:
        break

cap.release()
cv2.destroyAllWindows()
```

**Resultados cámara local**

<img src="https://drive.google.com/uc?id=1_5lwt27E4EZv9bmMecJ-FGSGsvxjMWQo" height="300px">

## 7. NPL<a name="id7"></a>
## 8. Aplicación web.<a name="id8"></a>

## 9. Conclusiones.<a name="id9"></a>
