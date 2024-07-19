# Segment Anything Model (SAM) para Procesamiento Digital de Imágenes en Sensores Remotos

## Descripción

El Segment Anything Model (SAM) es un modelo avanzado de segmentación de imágenes desarrollado por Meta. Este proyecto utiliza SAM para la segmentación y análisis de imágenes satelitales y de sensores remotos, permitiendo la identificación y clasificación precisa de diferentes características del terreno y objetos en las imágenes.

## Características

- **Segmentación Precisa**: Identificación y etiquetado detallado de características en imágenes satelitales.
- **Modelo Preentrenado**: Utiliza un modelo preentrenado robusto y versátil para diversas aplicaciones de teledetección.
- **Soporte para Imágenes Satelitales**: Compatible con diferentes tipos de imágenes de sensores remotos.
- **Conversión de TMS a GeoTIFF**: Facilita la conversión de mosaicos de teselas de mapas a archivos GeoTIFF.
- **Generación de GeoPackages**: Exporta las segmentaciones a formatos geoespaciales estándar como GeoPackage.

## Instalación

1. Clona este repositorio:

    ```bash
    git clone https://github.com/tu-usuario/tu-repositorio.git
    cd tu-repositorio
    ```

2. Instala las dependencias necesarias:

    ```bash
    pip install -r requirements.txt
    ```

## Uso

### Conversión de TMS a GeoTIFF

Convierte un mosaico de teselas de mapas a un archivo GeoTIFF utilizando una URL TMS y una caja delimitadora (bounding box).

```python
import os
from samgeo import SamGeo, tms_to_geotiff

# Define variables
image = 'colombia_maptiler_area.tif'
result = 'results_maptiler.gpkg'

# Define la caja delimitadora y el nivel de zoom
bbox = [9.719922, -75.129867, 9.721606, -75.127218]
zoom = 20

# URL del TMS
tms_url = 'https://api.maptiler.com/maps/satellite/{z}/{x}/{y}.jpg?key=TU_API_KEY'

# Convertir TMS a GeoTIFF
try:
    tms_to_geotiff(output=image, bbox=bbox, zoom=zoom, source=tms_url)
except Exception as e:
    print(f"An error occurred: {e}")

## Generación de Segmentación
### Usa SAM para generar una segmentación de la imagen y convertirla a GeoPackage.

# Inicializar SAM con el modelo preentrenado y parámetros
sam = SamGeo(
    checkpoint='sam_vit_h_4b8939.pth',  # Ruta al archivo del modelo
    model_type='vit_h',                 # Tipo de modelo
    device='cuda',                      # Dispositivo (puede ser 'cpu' o 'cuda')
    erosion_kernel=(3, 3),              # Parámetro de erosión para el procesamiento de imágenes
    mask_multiplier=255,                # Multiplicador para las máscaras
    sam_kwargs=None,
)

# Generar la segmentación de la imagen
try:
    sam.generate(image, 'segment_maptiler.tif')
    # Convertir la imagen segmentada a GeoPackage
    sam.tiff_to_gpkg('segment_maptiler.tif', result, simplify_tolerance=None)
except Exception as e:
    print(f"An error occurred during segmentation or GeoPackage conversion: {e}")

##Aplicaciones

###  Monitoreo Ambiental: Segmentación de áreas forestales, cuerpos de agua y zonas urbanas.
### Agricultura de Precisión: Identificación y clasificación de diferentes cultivos y su estado.
### Gestión de Desastres: Análisis de áreas afectadas por incendios, inundaciones u otros desastres naturales.
### Planificación Urbana: Evaluación del uso del suelo y desarrollo de infraestructuras.

## Contribuciones
Las contribuciones son bienvenidas. Por favor, realiza un fork del repositorio y envía un pull request con tus mejoras.

## Licencia
Este proyecto está bajo la Licencia MIT. Consulta el archivo LICENSE para más detalles.

## Contacto
Para cualquier pregunta o comentario, por favor contacta a alexanderariza@gmail.com.

