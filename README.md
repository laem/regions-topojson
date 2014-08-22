regions-topojson
================

Extract country regions lightweight topojson from openstreetmap. 

Example with french regions : https://github.com/laem/regions-topojson/blob/master/france/lala.json

Follow these steps to get topojson files for the other countries :

```
wget www.overpass-api.de/api/interpreter --post-file=my.xml -O my.osm
```

```
<osm-script timeout="1000">
  <!-- gather results -->
  <union>
    <!-- query part for: “admin_level=2” -->
    <query type="relation">
      <has-kv k="admin_level" v="4"/>
      <has-kv k="ISO3166-2" regv="FR-"/>
      
    </query>
  </union>
  <!-- print results -->
  <print mode="body"/>
  <recurse type="down"/>
  <print mode="skeleton" order="quadtile"/>
</osm-script>
```
```
npm install osmtogeojson
osmtogeojson my.osm > FR.geojson
```
You can stop here and use geojson, but files will be huge. Simplify them with topojson :

```
npm install topojson
topojson -o lala.json FR.geojson -q 10000 --simplify-proportion 0.02 -p
```
