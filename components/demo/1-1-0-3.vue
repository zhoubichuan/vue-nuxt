<template>
  <div class="map">
    <h4>EPSG:4326 全球通用</h4>
    <div id="map4326" style="height: 44%"></div>
    <h4>EPSG:3857 web 地图专用 openlayers 默认的</h4>
    <div id="map3857" style="height: 44%"></div>
  </div>
</template>

<script>
export default {
  mounted() {
    let {
      Feature,
      geom: {
        Polygon: { circular: circularPolygon },
      },
      Map,
      View,
      layer: { Tile: TileLayer, Vector: VectorLayer },
      source: { TileWMS, Vector: VectorSource },
    } = ol;
    const vectorLayer4326 = new VectorLayer({
      source: new VectorSource(),
    });

    const vectorLayer3857 = new VectorLayer({
      source: new VectorSource(),
    });

    const map4326 = new Map({
      layers: [
        new TileLayer({
          source: new TileWMS({
            url: "https://ahocevar.com/geoserver/wms",
            params: {
              LAYERS: "ne:NE1_HR_LC_SR_W_DR",
              TILED: true,
            },
          }),
        }),
        vectorLayer4326,
      ],
      target: "map4326",
      view: new View({
        projection: "EPSG:4326",
        center: [12579156, 3274244],
        zoom: 2,
      }),
    });

    const map3857 = new Map({
      layers: [
        new TileLayer({
          source: new TileWMS({
            url: "https://ahocevar.com/geoserver/wms",
            params: {
              LAYERS: "ne:NE1_HR_LC_SR_W_DR",
              TILED: true,
            },
          }),
        }),
        vectorLayer3857,
      ],
      target: "map3857",
      view: new View({
        center: [12579156, 3274244],
        zoom: 2,
      }),
    });

    const radius = 800000;
    let x, y;
    for (x = -180; x < 180; x += 30) {
      for (y = -90; y <= 90; y += 30) {
        const circle4326 = circularPolygon([x, y], radius, 64);
        const circle3857 = circle4326
          .clone()
          .transform("EPSG:4326", "EPSG:3857");
        vectorLayer4326.getSource().addFeature(new Feature(circle4326));
        vectorLayer3857.getSource().addFeature(new Feature(circle3857));
      }
    }
  },
};
</script>
<style>
.map {
  width: 100%;
  height: 400px;
}
</style>