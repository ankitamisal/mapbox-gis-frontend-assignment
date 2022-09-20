<template>

    <!-- <div class="mapboxgl-ctrl-geocoder mapboxgl-ctrl">
        <input type="text" class="mapboxgl-ctrl-geocoder--input" placeholder="Search" aria-label="Search">
        <div class="suggestions-wrapper">
            <ul class="suggestions"></ul>
        </div>
        <div class="mapboxgl-ctrl-geocoder--pin-right">
            <button aria-label="Clear" class="mapboxgl-ctrl-geocoder--button"></button>
        </div>
    </div> -->
    <div>
        <!-- <select id="layer-change">
            <option selected value="mapbox://styles/mapbox/streets-v11">Dark</option>
            <option value="mapbox://styles/mapbox/outdoors-v11">outdoors</option>
            <option value="mapbox://styles/mapbox/light-v10">light</option>
            <option value="mapbox://styles/mapbox/dark-v10">dark</option>
            <option value="mapbox://styles/mapbox/satellite-v9">satellite</option>
            <option value="mapbox://styles/mapbox/satellite-streets-v11">
                satellite-streets
            </option>
            <option value="mapbox://styles/mapbox/navigation-day-v1">
                navigation-day
            </option>
            <option value="mapbox://styles/mapbox/navigation-night-v1">
                navigation-night
            </option>
        </select> -->
    </div>
    <main class="w-screen h-screen">
        <v-map class="w-full h-full" :options="data.options" @loaded="onMapLoaded">
            <!-- <VLayerMapboxGeojson :sourceId="data.geoJsonSource.id" :source="data.geoJsonSource" layerId="myLayer"
                :layer="data.geoJsonlayer" /> -->
        </v-map>
    </main>
</template>
<script setup lang="ts">
import mapboxgl from "mapbox-gl";
import VMap from "v-mapbox";
import MapboxGeocoder from '@mapbox/mapbox-gl-geocoder';
import '@mapbox/mapbox-gl-geocoder/dist/mapbox-gl-geocoder.css';
// function upload() {
//     console.log("hii");
// }
const data = reactive({
    options: {
        accessToken:
            "pk.eyJ1IjoibWF5dXJ3YWtpa2FyIiwiYSI6ImNsNmdjdGxwbjBiNGMzY282bWh0dng2c2kifQ.y-m4-zQKOeOOnDG5I1u6ng",
        style: "mapbox://styles/mapbox/outdoors-v11",
        center: [-68.137343, 45.137451],
        zoom: 8,
        maxZoom: 22,
        crossSourceCollisions: false,
        failIfMajorPerformanceCaveat: false,
        attributionControl: false,
        preserveDrawingBuffer: true,
        hash: false,
        minPitch: 0,
        maxPitch: 60,
        container: 'map'
    } as mapboxgl.MapboxOptions,
    map: {} as mapboxgl.Map,
    // geoJsonSource: {
    //     'id': 'maine',
    //     'type': 'geojson',
    //     'data': {
    //         'type': 'Feature',
    //         'geometry': {
    //             'type': 'Polygon',
    //             'coordinates': [
    //                 [
    //                     [-67.13734, 45.13745],
    //                     [-66.96466, 44.8097],
    //                     [-68.03252, 44.3252],
    //                     [-69.06, 43.98],
    //                     [-70.11617, 43.68405],
    //                     [-70.64573, 43.09008],
    //                     [-70.75102, 43.08003],
    //                     [-70.79761, 43.21973],
    //                     [-70.98176, 43.36789],
    //                     [-70.94416, 43.46633],
    //                     [-71.08482, 45.30524],
    //                     [-70.66002, 45.46022],
    //                     [-70.30495, 45.91479],
    //                     [-70.00014, 46.69317],
    //                     [-69.23708, 47.44777],
    //                     [-68.90478, 47.18479],
    //                     [-68.2343, 47.35462],
    //                     [-67.79035, 47.06624],
    //                     [-67.79141, 45.70258],
    //                     [-67.13734, 45.13745]
    //                 ]
    //             ]
    //         }
    //     }
    // },
    // geoJsonlayer: {
    //     'id': 'myLayer',
    //     'type': 'fill',
    //     'source': 'maine', // reference the data source
    //     'layout': {},
    //     'paint': {
    //         'fill-color': 'black', // blue color fill
    //         'fill-opacity': 0.5
    //     }
    // }
});

async function onMapLoaded(map: mapboxgl.Map) {
    // let colorSet = 0;
    // let color = [
    //     "051E10",
    //     "#F92929",
    //     "#21F728",
    //     "#D4F721",
    //     "#F76821",
    //     "#F721BE",
    //     "#5321F7",
    //     "#21F77E",
    //     "#051E10",
    // ];


    // map.addControl(
    //     new MapboxGeocoder({
    //     accessToken: mapboxgl.accessToken,
    //     mapboxgl: mapboxgl
    //     })
    //     );

    data.map = map;

    mapboxgl.accessToken = 'pk.eyJ1IjoibWF5dXJ3YWtpa2FyIiwiYSI6ImNsNmdjdGxwbjBiNGMzY282bWh0dng2c2kifQ.y-m4-zQKOeOOnDG5I1u6ng';

    const geocoder = new MapboxGeocoder({
        accessToken: mapboxgl.accessToken,
        mapboxgl: mapboxgl
    });

    data.map.addControl(geocoder);

    // console.log('on map laoded: ', map);

    data.map.addSource('pune', {
        'type': 'geojson',
        'data': {
            'type': 'Feature',
            'geometry': {
                'type': 'Polygon',
                'coordinates': [
                    [
                        [
                            73.68942260742188,
                            18.530398219358684
                        ],
                        [
                            73.65509033203125,
                            18.340187242207897
                        ],
                        [
                            73.99154663085938,
                            18.359739156553683
                        ],
                        [
                            73.99429321289062,
                            18.641040231399984
                        ],
                        [
                            73.68942260742188,
                            18.530398219358684
                        ]
                    ]
                ]
            }
        }
    });

    // Add a new layer to visualize the polygon.
    data.map.addLayer({
        'id': 'pune',
        'type': 'fill',
        'source': 'pune', // reference the data source
        'layout': {},
        'paint': {
            'fill-color': 'black', // blue color fill
            'fill-opacity': 0.5
        }
    });



    // console.log(map);
    // const marker = new mapboxgl.Marker({
    //     draggable: true,
    //     color: "#000000",
    // });
    // marker.setLngLat([-122.4473, 37.7535]);
    // marker.addTo(map);
    // map.on("click", (e) => {
    //     colorSet++;
    //     console.log(e.lngLat.lat, e.lngLat.lng);
    //     if (colorSet === color.length) {
    //         colorSet = 0;
    //         new mapboxgl.Marker({
    //             draggable: true,
    //             color: `${color[colorSet]}`,
    //         })
    //             .setLngLat([e.lngLat.lng, e.lngLat.lat])
    //             .addTo(map);
    //     }
    //     new mapboxgl.Marker({
    //         draggable: true,
    //         color: `${color[colorSet]}`,
    //     })
    //         .setLngLat([e.lngLat.lng, e.lngLat.lat])
    //         .addTo(map);
    // });
    // const setStyle: any = document.getElementById("layer-change");
    // setStyle.addEventListener("change", (event) => {
    //     console.log(event);
    //     map.setStyle(event.target.value);
    // });
    // map.flyTo({
    //     center: [75.70610046386717, 18.333587971760853],
    //     zoom: 9,
    //     speed: 0.9,
    //     curve: 1,
    //     easing(t) {
    //         return t;
    //     },
    // });

    // map.addLayer({
    //     id: 'points-of-interest',
    //     source: {
    //         type: 'vector',
    //         url: 'mapbox://mapbox.mapbox-streets-v8'
    //     },
    //     'source-layer': 'poi_label',
    //     type: 'circle',
    //     paint: {
    //         // Mapbox Style Specification paint properties
    //     },
    //     layout: {
    //         // Mapbox Style Specification layout properties
    //     }
    // }),


    // map.on('load', () => {
    //     // Add a data source containing GeoJSON data.
    //     map.addSource('maine', {
    //         'type': 'geojson',
    //         'data': {
    //             'type': 'Feature',
    //             'geometry': {
    //                 'type': 'Polygon',
    //                 // These coordinates outline Maine.
    //                 'coordinates': [
    //                     [
    //                         [-67.13734, 45.13745],
    //                         [-66.96466, 44.8097],
    //                         [-68.03252, 44.3252],
    //                         [-69.06, 43.98],
    //                         [-70.11617, 43.68405],
    //                         [-70.64573, 43.09008],
    //                         [-70.75102, 43.08003],
    //                         [-70.79761, 43.21973],
    //                         [-70.98176, 43.36789],
    //                         [-70.94416, 43.46633],
    //                         [-71.08482, 45.30524],
    //                         [-70.66002, 45.46022],
    //                         [-70.30495, 45.91479],
    //                         [-70.00014, 46.69317],
    //                         [-69.23708, 47.44777],
    //                         [-68.90478, 47.18479],
    //                         [-68.2343, 47.35462],
    //                         [-67.79035, 47.06624],
    //                         [-67.79141, 45.70258],
    //                         [-67.13734, 45.13745]
    //                     ]
    //                 ]
    //             }
    //         }
    //     });

    //     // Add a new layer to visualize the polygon.
    //     map.addLayer({
    //         'id': 'maine',
    //         'type': 'fill',
    //         'source': 'maine', // reference the data source
    //         'layout': {},
    //         'paint': {
    //             'fill-color': 'black', // blue color fill
    //             'fill-opacity': 0.5
    //         }
    //     });
    // });





}
</script>
<style>
#layer-change {
    position: fixed;
    top: 10px;
    left: 10px;
    z-index: 1;
}

#file {
    width: 185px;
    position: fixed;
    top: 40px;
    right: 5px;
    z-index: 1;
}

#search {
    position: fixed;
    top: 10px;
    right: 10px;
    z-index: 1;
}

html,
body {
    margin: 0;
}

#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2C3E50;
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
</style>
  