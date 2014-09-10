regions-topojson
================

Example with french regions : https://github.com/laem/regions-topojson/blob/master/france/lala.json

Follow these steps to get them for other countries :

Run this command...
```
wget www.overpass-api.de/api/interpreter --post-file=my.xml -O my.osm
```

...using this my.xml file. 

Just change FR for DE to get german regions for example, and check http://wiki.openstreetmap.org/wiki/Tag:boundary%3Dadministrative to get the 'admin_level' that you need. 

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

Unfortunately, not all country regions on OSM are marked with the ISO3166-2 attribute. E.g. English regions will have to be retrieved using this query lines instead : 

```
<has-kv k="admin_level" v="5"/>
<has-kv k="ref:nuts:1" regv="UK"/>
```

Then : 

```
npm install osmtogeojson
osmtogeojson my.osm > MYREGIONS.geojson
```
You can stop here and use geojson, but files will be huge. Simplify them with topojson :

```
npm install topojson
topojson -o lala.json MYREGIONS.geojson -q 10000 --simplify-proportion 0.02 -p
```


Developed for [qunb.com](https://www.qunb.com)
