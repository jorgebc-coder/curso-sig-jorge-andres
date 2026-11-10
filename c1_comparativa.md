# C1. Comparativa de estándares y servicios geográficos

El objetivo de esta actividad es reconocer las diferencias entre los servicios geográficos WMS, WFS, WMTS, Esri FeatureServer y OGC API – Features, mediante la revisión de sus características y la consulta de servicios reales.

Se analizaron los servicios del Geoportal del DANE, un servicio FeatureServer de veredas de Boyacá y una colección de demostración de OGC API – Features.

## 2. Comparación de servicios

| Servicio | Información que entrega | Operación o consulta principal | Formato o respuesta |
|---|---|---|---|
| WMS | Mapas representados como imágenes. | GetMap y GetCapabilities. | PNG, JPEG, TIFF, entre otros. |
| WFS | Entidades geográficas con geometrías y atributos. | GetFeature. | Datos vectoriales. |
| WMTS | Mapas organizados en teselas o mosaicos previamente renderizados. | GetTile. | Teselas de mapas. |
| Esri FeatureServer | Entidades vectoriales y sus atributos mediante una API REST. | `/query` | JSON y GeoJSON, entre otros. |
| OGC API – Features | Entidades geográficas y sus atributos mediante una API REST basada en estándares OGC. | `/collections/{id}/items` | GeoJSON y otros formatos. |

## 3. Consulta del servicio WMS del DANE

Se consultó el servicio WMS del Geoportal del DANE mediante la solicitud GetCapabilities:

https://geoportal.dane.gov.co/mparcgis/services/MGN2024/Serv_CapasMGN_2024/MapServer/WMSServer?service=WMS&request=GetCapabilities

La respuesta corresponde a un documento XML que describe las capacidades del servicio. En la revisión se identificaron diez capas geográficas:

- Manzana.
- Sección Urbana.
- Sector Urbano.
- Sección Rural.
- Sector Rural.
- Clase.
- Zona Urbana.
- Municipio.
- Departamento.
- País.

También se identificaron los sistemas de referencia CRS:84, EPSG:4326, EPSG:3857 y EPSG:102100.

Entre los formatos disponibles para la representación de mapas mediante GetMap se encontraron BMP, JPEG, TIFF, PNG, PNG8, PNG24, PNG32, GIF y SVG.

Esta consulta permitió reconocer que WMS publica información geográfica como imágenes cartográficas. GetCapabilities permite conocer las capas y capacidades del servicio, mientras que GetMap solicita la representación del mapa.

## 4. Consulta del servicio Esri FeatureServer

Se consultó el servicio de veredas de Boyacá publicado mediante ArcGIS REST API:

https://services6.arcgis.com/Ex2fmFOuTOHRZtML/ArcGIS/rest/services/Veredas_Boyaca_2024/FeatureServer/0/query?where=1%3D1&outFields=*&resultRecordCount=3&f=geojson

La consulta utiliza los siguientes parámetros:

- `where`: establece la condición de consulta. En este caso, `1=1` permite consultar todos los registros.
- `outFields`: define los campos que se desean obtener. El valor `*` solicita todos los atributos.
- `resultRecordCount`: establece el número de registros solicitado.
- `f`: define el formato de respuesta; en este caso, GeoJSON.

La respuesta presenta una estructura FeatureCollection, que contiene las entidades dentro de `features`. Cada entidad incluye su geometría (`geometry`) y sus atributos (`properties`).

En la consulta se identificaron geometrías de tipo Polygon y atributos como nombre de la vereda, municipio, departamento, código, área, vigencia y fuente de información.

La paginación en este tipo de servicio puede realizarse mediante parámetros como `resultOffset` y `resultRecordCount`, que permiten consultar registros por bloques.

## 5. Consulta del servicio OGC API – Features

Se consultó el servicio de demostración pygeoapi:

https://demo.pygeoapi.io/master/collections?f=json

En la respuesta se identificó la colección `obs`, denominada Observations. Posteriormente, se consultaron sus entidades mediante la siguiente dirección:

https://demo.pygeoapi.io/master/collections/obs/items?limit=3&f=json

La respuesta contiene una estructura FeatureCollection con tres entidades. Se identificaron los siguientes elementos:

- `type`: identifica el documento como una colección de entidades.
- `features`: contiene los registros geográficos consultados.
- `geometry`: contiene la geometría de cada entidad; en este ejemplo, de tipo Point.
- `coordinates`: almacena las coordenadas geográficas.
- `properties`: contiene los atributos, como identificador, estación, fecha y valor.
- `numberMatched`: indica el número de registros coincidentes.
- `numberReturned`: indica el número de registros devueltos.
- `links`: contiene enlaces para acceder a otros formatos y continuar la consulta.

La respuesta indicó cinco registros coincidentes y tres registros devueltos. También incluyó un enlace `next`, que permite continuar la consulta mediante el parámetro `offset`.

En este servicio, el parámetro `limit` permite establecer el número máximo de entidades solicitadas, mientras que `offset` permite avanzar en los registros para realizar la paginación.

## 6. Comparación de filtrado y paginación

| Servicio | Filtrado | Paginación |
|---|---|---|
| WMS | Mediante parámetros de solicitud y selección de capas; no funciona como una consulta de atributos vectoriales convencional. | No se aplica como paginación de entidades. |
| WFS | Mediante operaciones y filtros de consulta, como GetFeature. | Depende de las capacidades y parámetros de la implementación del servicio. |
| Esri FeatureServer | Mediante el parámetro `where`, que permite establecer condiciones sobre los atributos. | Mediante `resultRecordCount` y `resultOffset`. |
| OGC API – Features | Mediante parámetros de consulta y filtros compatibles con la implementación. | Mediante `limit` y `offset`, además de los enlaces de navegación, como `next`. |

## 7. Conclusiones

La revisión permitió identificar diferencias entre los servicios geográficos según el tipo de información que publican y los mecanismos utilizados para consultarla.

WMS está orientado a la representación cartográfica mediante imágenes, mientras que WFS permite consultar entidades vectoriales con sus geometrías y atributos. Por su parte, Esri FeatureServer y OGC API – Features ofrecen acceso a entidades geográficas mediante interfaces REST y respuestas estructuradas.

En las consultas realizadas, FeatureServer permitió acceder a información vectorial de veredas de Boyacá, representadas mediante polígonos y atributos asociados. OGC API – Features permitió consultar registros de observaciones representados mediante puntos, con información de estaciones, fechas y valores.

Aunque ambos servicios permiten obtener información geográfica en formato GeoJSON, presentan diferencias en sus rutas de consulta y mecanismos de paginación. Esta comparación permite reconocer la importancia de seleccionar el servicio geográfico de acuerdo con el tipo de información y las necesidades de consulta de cada aplicación SIG.
