# Praxisbezogene geographische Datenanalyse: Untersuchung von Unfallzahlen in Berlin in QGIS

## 1. Erwerb von Open-Source-Daten
Die Daten zu Unfallorten für das Jahr 2022 in Deutschland im Shapefile-Format und die Geojson-Daten der Berliner Stadtgrenzen wurden als Open Source / Freie Nutzung erworben.

## 2. Angleichung der Koordinatensysteme
Die erworbenen Daten (Unfallorte.shp) waren im UTM32-Koordinatensystem, während die Berliner Stadtgrenze im WGS84-Koordinatensystem vorlag. Um die Daten gemeinsam verarbeiten zu können, wurden die Unfallortdaten von UTM32 auf WGS84 konvertiert. Dies wurde in QGIS durchgeführt mit exportieren Feature in WGS84 Koordinatensystem. Das WGS84-Koordinatensystem ermöglicht eine einfache Darstellung dieser Daten auf Webseiten, da Karten und Navigationsdienste im Internet mit dem WGS84 (GPS)-Koordinatensystem kompatibel sind.

## 3. Import der Daten in eine PostgreSQL-Datenbank
Die räumliche Daten können mittels der PostGIS in eine PostgreSQL-Datenbank importiert werden. Dafür eine Konversation vom shapefile auf SQL benötigt. In der Datenbank befinden sich 256.492 Punkte (Point Features) mit 22 Feldern.


```bash

shp2pgsql -s 25382 -I Unfallorte.shp public.Unfallorte | psql -U postgres -d accident_places
```
-s 25832 definiert Koordinatensystem von Input Daten was ist UTM32 in diesem Fall.
-d accident_places ist Datenbankname in PostgreSQL

## 4. Datenanalyze in QGIS
### 4.1. Clipping der Daten in Berlin
Mithilfe des Clip-Tools in QGIS wurden die Features innerhalb der Berliner Stadtgrenzen ausgeschnitten. Dieser Schritt resultierte in 12.336 Punkten, die Unfälle in Berlin repräsentieren.

## 4.2. Erstellung von Rastern auf dem Berliner Polygon und Zählung der Unfälle in jeder Zelle
Mit dem Tool zur Erstellung von Rastern können Raster auf dem Berliner Polygon erstellt werden. Das Tool "Count Points in Polygon" ermöglicht es uns, die Anzahl der Unfälle in jeder Zelle zu ermitteln.

## 5. Darstellung der Ergebnnissen
Das Feld NUMBERPOINTS zeigt, dass die Anzahl der Unfälle zwischen den Gitterzellen zwischen 10 und 650 variiert. Mit dem Symbology-Tool und entsprechenden Abfragen können wir die Ergebnisse der durchgeführten Analyse deutlich visualisieren.
