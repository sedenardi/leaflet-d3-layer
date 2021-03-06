# leaflet-d3-layer
utilizes [Mike Bostock's excellent instructions](http://bost.ocks.org/mike/leaflet/) to use D3 to render GeoJSON features on a Leaflet Map

logic is wrapped into an object that extends Leaflet's own GeoJSON layer, so its easy to add to a map using Leaflet's usual pattern:

    var d3Layer = new L.GeoJSON.d3({...insert geojson here...}, options);
    var map = new L.Map("map").addLayer(d3Layer);
    
**L.GeoJSON.d3.async** allows you to create a layer that references a geojson data service, such as a WFS provided by Geoserver.

    var asyncD3Layer = new L.GeoJSON.d3.async("http:/url/for/geoserver/wfs");
    var map = new L.Map("map").addLayer(asyncD3Layer)

#### options
- `classed` - Objects applied using `d3`'s [`classed`](https://github.com/mbostock/d3/wiki/Selections#classed) function.

Specifying a `classed` object in the options 

    var d3Layer = new L.GeoJSON.d3(geoJson, {
      classed: {
        feature: true,
        selected: function(d,i) { return d.properties.selected; }
      }
    });

is the equivalent of performing the following options on the paths directly.

    d3.classed('feature', true);
    d3.classed('selected', function(d,i) { return d.properties.selected; })

- `attr` - Objects applied using the [`attr`](https://github.com/mbostock/d3/wiki/Selections#attr) function. Useful for setting properties that have no CSS equivalent. See above for use.
- `on` - Objects applied using the [`on`](https://github.com/mbostock/d3/wiki/Selections#on) function. Useful for setting event listeners. See above for use.
- `idFunction` - If your GeoJSON data has several features you need a unique identifier for each one. By default `leaflet-d3-layer` will look for an `id` field at the root of your feature. Use this option if your features have some other unique identifier.

Example: your feature has an `identifier` field in the properties member.

    geoJson = {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [125.6, 10.1]
      },
      "properties": {
        "name": "Dinagat Islands",
        "identifier": "point-dinagat"
      }
    }

You would then use the `idFunction` option like so:

    var d3Layer = new L.GeoJSON.d3(geoJson, {
      idFunction: function(d) { return d.properties.identifier; }
    });

## example application
check out `/dist/index.html`

## still working...
- there is definitely a better way to handle adding / removing data from the map using (D3's update patterns)[http://bl.ocks.org/mbostock/3808218]
- is this a good way to handle unique-value symbology? **SOLVED - use `idFunction` option**
- probably "async" layer should be branded and focused for what it is: a connector to a Geoserver WFS, although it would work right now for any GeoJSON data service that accepts a bbox query paramter
