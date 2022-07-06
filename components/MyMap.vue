<script setup>
import { ref } from 'vue'
import { fromLonLat } from 'ol/proj';
import hereIcon from '~/assets/placeholder.png'

const center = ref(fromLonLat([4, 51]))
const projection = ref('EPSG:3857')
const zoom = ref(8)
const rotation = ref(0)

const view = ref(null)
const map = ref(null)

const strokeWidth = ref(10)
const strokeColor = ref('red')

const coordinate = ref(fromLonLat([4.5, 50.5]))
const polygon = ref([[fromLonLat([4, 51]), fromLonLat([4, 50]), fromLonLat([5, 50]), fromLonLat([5, 51]), fromLonLat([4, 51])]])

const changeLocation = () => {
  coordinate.value = [coordinate.value[0] + 1000, coordinate.value[1] + 1000];
  view.value.fit([coordinate.value[0], coordinate.value[1], coordinate.value[0], coordinate.value[1]], {
    maxZoom: 8,
  })
}


</script>

<template>
  <button @click="changeLocation">changecoor</button>
  <ol-map ref="map" :loadTilesWhileAnimating="true" :loadTilesWhileInteracting="true" style="height:600px">
    <ol-view ref="view" :center="center" :rotation="rotation" :zoom="zoom" :projection="projection" />
    <ol-tile-layer>
      <ol-source-osm />
    </ol-tile-layer>

    <ol-geolocation :projection="projection">
      <template v-slot="slotProps">
        <ol-vector-layer :zIndex="2">
          <ol-source-vector>
            <ol-feature ref="positionFeature">
              <ol-geom-point :coordinates="slotProps.position"></ol-geom-point>
              <ol-style>
                <ol-style-icon :src="hereIcon" :scale="0.05"></ol-style-icon>
              </ol-style>
            </ol-feature>
          </ol-source-vector>
        </ol-vector-layer>
      </template>
    </ol-geolocation>

    <ol-vector-layer>
      <ol-source-vector>
        <ol-feature>
          <ol-geom-polygon :coordinates="polygon">
          </ol-geom-polygon>
          <ol-style>
            <ol-style-stroke :color="strokeColor" :width="strokeWidth"></ol-style-stroke>
          </ol-style>
        </ol-feature>

      </ol-source-vector>

    </ol-vector-layer>

    <ol-vector-layer>
      <ol-source-vector>
        <ol-feature>
          <ol-geom-point :coordinates="coordinate"></ol-geom-point>
          <ol-style>
            <ol-style-icon :src="hereIcon" :scale="0.05"></ol-style-icon>
          </ol-style>
        </ol-feature>

      </ol-source-vector>

    </ol-vector-layer>
  </ol-map>
  <div class="scss-test">
    <p>hELLO WORLD </p>
  </div>
</template>

<style scoped lang="scss">
@import 'vue3-openlayers/dist/vue3-openlayers.css';
.scss-test p{
  min-height: 200px;
  color: blue;
}
</style>
