<template>
  <main class="w-screen h-screen">
    <!-- <div id="map"></div> -->
    <v-map class="w-full h-full" :options="state.map" @loaded="onMap">
      <div id="menu">
        <select id="layer-change">
          <option
            id="satellite-v9"
            type="checkbox"
            name="style1"
            value="satellite-v9"
            checked="checked"
          >
            satellite
          </option>
          <option id="dark-v10" type="checkbox" name="style2" value="dark-v10">
            dark
          </option>
          <option
            id="light-v10"
            type="checkbox"
            name="style3"
            value="light-v10"
          >
            light
          </option>
          <option
            id="streets-v11"
            type="checkbox"
            name="style4"
            value="streets-v11"
          >
            streets
          </option>
          <option
            id="outdoors-v11"
            type="checkbox"
            name="style5"
            value="outdoors-v11"
          >
            outdoors
          </option>
        </select>
      </div>
    </v-map>
  </main>
</template>
<script setup lang="ts">
// display map on Browser......................................
import mapboxgl from "mapbox-gl";
import MapboxGeocoder from "@mapbox/mapbox-gl-geocoder";
import VMap from "v-mapbox";
mapboxgl.accessToken =
  "pk.eyJ1Ijoic29jaWFsZXhwbG9yZXIiLCJhIjoiREFQbXBISSJ9.dwFTwfSaWsHvktHrRtpydQ";
const state = reactive({
  map: {
    container: "map",
    style: "mapbox://styles/mapbox/streets-v11?optimize=true",
    center: [73.856743, 18.52043] as number[],
    zoom: 6,
    maxZoom: 22,
  },
});
// To get Backend point in frontend.....................................
let mapData: any = await $fetch("http://localhost:3000/map/");
console.log("map data", mapData);
// add different style on map ..................................
function onMap(map: mapboxgl.Map) {
  const layerToggle: any = document.getElementById("layer-change");
  layerToggle.addEventListener("change", (event) => {
    console.log(event);
    map.setStyle("mapbox://styles/mapbox/" + event.target.value);
  });
  //search bar used geocoder to display search bar.........
  map.addControl(
    new MapboxGeocoder({
      accessToken: mapboxgl.accessToken,
      mapboxgl: mapboxgl,
    })
  );
  //marker..................................
  mapData.map((ele) => {
    new mapboxgl.Marker({
      draggable: true,
      color: "#" + (Math.random().toString(16) + "000000").substring(2, 8),
    })
      .setLngLat([ele.lat, ele.lon])
      .addTo(map);
  });
  // add polygon on pune location..........................
  map.addSource("pune", {
    type: "geojson",
    data: {
      type: "Feature",
      geometry: {
        type: "Polygon",
        coordinates: [
          [
          [
              72.85171508789062,
              19.08547999705885
            ],
            [
              72.84347534179688,
              19.02057711096681
            ],
            [
              72.91351318359375,
              19.073799352002716
            ],
            [
              72.85171508789062,
              19.08547999705885
            ]
          ],
        ],
      },
    },
  });
  map.addLayer({
    id: "pune",
    type: "fill",
    source: "pune", // reference the data source
    layout: {},
    paint: {
      "fill-color": " gray", // blue color fill
      "fill-opacity": 1,
    },
  });
  // fly the map.........................................
  map.flyTo({
    center: [73.8743, 19.2032],
    zoom: 9,
    speed: 0.9,
    curve: 1,
    easing(t) {
      return t;
    },
  });
}
</script>
<style>
body {
  margin: 0;
  padding: 0;
}
#menu {
  position: absolute;
  background: #EFEFEF;
  padding: 10px;
  font-family: "Open Sans", sans-serif;
}
.w-screen {
  width: 100vw;
}
.h-screen {
  height: 100vh;
}
.h-full {
  height: 100%;
}
.w-full {
  width: 100%;
}
.mapboxgl-popup {
  max-width: 400px;
  font: 12px/20px "Helvetica Neue", Arial, Helvetica, sans-serif;
}
</style>