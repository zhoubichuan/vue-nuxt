<template>
  <div ref="map" class="map"></div>
</template>
<script>
export default {
  mounted() {
    let {
      Map,
      View,
      layer: { WebGLTile: TileLayer },
      source: { OSM },
      control: { Zoom, defaults },
    } = ol
    new Map({
      target: this.$refs.map,
      layers: [
        new TileLayer({
          source: new OSM(),
        }),
      ],
      view: new View({
        center: [12579156, 3274244],
        zoom: 12,
      }),
      controls: defaults().extend([
        new Zoom({
          zoomInLabel: "增",
        }),
      ]),
    })
  },
}
</script>
<style>
.add-zoom {
  color: red;
}
</style>