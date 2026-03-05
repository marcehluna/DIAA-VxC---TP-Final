# Arquitectura del notebook Ejercicio_Final

Diagrama del flujo y componentes del código del notebook de detección de emociones (Visión por Computadora).

```mermaid
flowchart TB
    subgraph SETUP["1. Configuración inicial"]
        A1["pip install opencv-python"]
        A2["Montar Google Drive\n(drive.mount)"]
        A1 --> A2
    end

    subgraph DATOS["2. Carga y preprocesamiento de datos"]
        B1["Entrada usuario:\n- Ruta carpeta emociones\n- Ancho/alto objetivo"]
        B2["Recorrer subcarpetas\n(una por emoción)"]
        B3["Por cada imagen:\n- cv2.imread\n- Escala de grises\n- Redimensionar"]
        B4["Salidas:\nprocessed_images\nimage_filenames\nimage_labels"]
        B1 --> B2 --> B3 --> B4
    end

    subgraph DETECTOR["3. Detección de rostros"]
        C1["Usuario elige algoritmo"]
        C2{"Opción?"}
        C3["OpenCV ResNet-10\n(deploy.prototxt +\n.caffemodel)"]
        C4["MediaPipe\nFaceDetection"]
        C5["face_detector listo"]
        C1 --> C2
        C2 -->|"1"| C3 --> C5
        C2 -->|"2"| C4 --> C5
    end

    SETUP --> DATOS
    DATOS --> DETECTOR

    style SETUP fill:#e8f4f8
    style DATOS fill:#f0f8e8
    style DETECTOR fill:#f8f0e8
```

## Resumen de bloques

| Bloque | Celdas | Responsabilidad |
|--------|--------|-----------------|
| **Configuración inicial** | 1–3 | Dependencias (OpenCV), montaje de Google Drive para acceder a datos y modelos. |
| **Carga y preprocesamiento** | 5–6 | Ruta y dimensiones por usuario; carga imágenes por carpeta de emoción; conversión a escala de grises y redimensionado; salidas: `processed_images`, `image_filenames`, `image_labels`. |
| **Detección de rostros** | 7–8 | Elección de algoritmo (1: OpenCV ResNet-10, 2: MediaPipe); carga del modelo correspondiente; objeto `face_detector` listo para uso posterior. |

## Dependencias externas

- **opencv-python**: lectura de imágenes, preprocesamiento (escala de grises, resize), DNN ResNet-10.
- **google.colab.drive**: acceso a datos y archivos de modelo en Drive.
- **mediapipe** (si se elige opción 2): detección de rostros con MediaPipe.

## Flujo de datos

```
Google Drive (carpetas por emoción)
    → Imágenes en disco
    → Preprocesamiento (gris, resize)
    → processed_images, image_labels
    → [Uso posterior: face_detector + clasificador de emociones]
```
