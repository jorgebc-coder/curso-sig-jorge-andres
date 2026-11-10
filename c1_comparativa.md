# C1. Comparativa de servicios geográficos

## 1. WMS del DANE

- **Capas publicadas:** 10.
- **CRS soportados:** CRS:84, EPSG:4326, EPSG:3857 y EPSG:102100.
- **Formatos de salida:** PNG, JPEG, TIFF, GIF, BMP y SVG, entre otros.

## 2. Comparación de respuestas JSON

**Esri FeatureServer**

- Formato: GeoJSON.
- Estructura: `FeatureCollection`, `features`, `geometry` y `properties`.
- Geometría consultada: Polygon.
- Atributos: nombre de vereda, municipio, departamento, código y área.
- Consulta: `query`.
- Paginación: `resultRecordCount` y `resultOffset`.

**OGC API – Features (pygeoapi)**

- Formato: GeoJSON.
- Estructura: `FeatureCollection`, `features`, `geometry` y `properties`.
- Geometría consultada: Point.
- Atributos: identificador, estación, fecha y valor.
- Registros encontrados: 5.
- Registros devueltos: 3.
- Paginación: `limit` y `offset`, con enlace `next`.

## 3. Tabla comparativa

| Servicio | ¿Qué entrega? | ¿Cómo se filtra? | ¿Cómo se pagina? |
|---|---|---|---|
| WMS | Mapas en imágenes. | Selección de capas y parámetros de solicitud. | No aplica paginación de entidades. |
| WFS | Entidades vectoriales con geometría y atributos. | GetFeature y filtros. | Según las capacidades del servicio. |
| FeatureServer | Entidades vectoriales en JSON o GeoJSON. | Parámetro `where`. | `resultRecordCount` y `resultOffset`. |
| OGC API – Features | Entidades geográficas y atributos. | Parámetros de consulta y filtros compatibles. | `limit` y `offset`. |
