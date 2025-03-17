Estuve mirando diferentes fuentes de datos para datos geograficos de madrid.

OpenStreetMap parece tener bastante informacion con muchos puntos y se va actualizando cada X dias mientras que tambien mantiene algun snapshot pasado

He encontrado una manera de extraer la informacion facilmente con dumps desde:

https://download.geofabrik.de/europe/spain/madrid.html#


Para renderizar el dato, tengo aun dudas. Por lo que entiendo, necesito un TileServer y luego el OSM es un archivo (por lo general en formato PBF) que entiendo contiene informacion sobre las entidades

https://wiki.openstreetmap.org/wiki/PBF_Format

---------------

Ideas:

* Definir los distritos de madrid.
* Tener informacion general del ayuntamiento sobre los mismos
* Cruzar esa informacion con los puntos (a poder ser de forma historica) para ver si hay patrones.


---------------

Para renderizar, segun deepseek, hay varias ideas posibles, una que parece tener sentido es :

osm2pgsql + PostGIS + TileServer GL


* https://osm2pgsql.org/
* https://github.com/maptiler/tileserver-gl?tab=readme-ov-file

Por ultimo, para usarlo en un jupyter notebook, necesitariamos

https://ipyleaflet.readthedocs.io/en/latest/

PAra conseguir las coordenadas de las diferentes municipios, habria que usar GADM:

https://gadm.org/data.html

Si queremos hacer custom boundaries, tendriamos que hacer con GEOJSON data en el momento de
renderizar con ipyleaflet o folium

```python
from ipyleaflet import Map, TileLayer, GeoJSON

# Create a map centered on Madrid
m = Map(center=(40.4168, -3.7038), zoom=12)

# Add OSM tiles
tile_layer = TileLayer(
    url='https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
    attribution='OpenStreetMap'
)
m.add_layer(tile_layer)

# Add district boundaries (GeoJSON)
geojson_layer = GeoJSON(
    data=districts_data,  # Replace with your GeoJSON data
    style={'color': 'blue', 'weight': 2, 'fillOpacity': 0.1},
    name='Districts'
)
m.add_layer(geojson_layer)

# Display the map
m
```