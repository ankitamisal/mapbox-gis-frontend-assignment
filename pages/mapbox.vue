<template>

    <head>
        <link rel='stylesheet'
            href='https://api.mapbox.com/mapbox-gl-js/plugins/mapbox-gl-draw/v1.3.0/mapbox-gl-draw.css'
            type='text/css' />
    </head>

    <div>
        <select id="layer-change">
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
        </select>
    </div>
    <main class="w-screen h-screen">
        <v-map class="w-full h-full" :options="data.options" @loaded="onMapLoaded">
            <!-- <VLayerMapboxGeojson :sourceId="data.geoJsonSource.id" :source="data.geoJsonSource" layerId="myLayer"
                :layer="data.geoJsonlayer" /> -->
            <div class="pre">
                <pre id="info"></pre>
            </div>
        </v-map>
    </main>
</template>
<script setup lang="ts">
import mapboxgl from "mapbox-gl";
import VMap from "v-mapbox";
import MapboxGeocoder from '@mapbox/mapbox-gl-geocoder';
import '@mapbox/mapbox-gl-geocoder/dist/mapbox-gl-geocoder.css';
import MapboxDraw from "@mapbox/mapbox-gl-draw";
// function upload() {
//     console.log("hii");
// }
// To get Backend point in frontend.

const data = reactive({
    options: {
        accessToken:
            "pk.eyJ1IjoibWF5dXJ3YWtpa2FyIiwiYSI6ImNsNmdjdGxwbjBiNGMzY282bWh0dng2c2kifQ.y-m4-zQKOeOOnDG5I1u6ng",
        style: "mapbox://styles/mapbox/outdoors-v11",
        center: [73.856743, 18.52043] as number[],
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


    // 
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




    data.map = map;

    mapboxgl.accessToken = 'pk.eyJ1IjoibWF5dXJ3YWtpa2FyIiwiYSI6ImNsNmdjdGxwbjBiNGMzY282bWh0dng2c2kifQ.y-m4-zQKOeOOnDG5I1u6ng';

    const geocoder = new MapboxGeocoder({
        accessToken: mapboxgl.accessToken,
        mapboxgl: mapboxgl
    });

    data.map.addControl(geocoder);
    const isMoving = map.isMoving();
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


    let mapData: any = await $fetch("http://localhost:3001/mapbox/");
    console.log("map data", mapData);
    function onMap() {
        const layerToggle: any = document.getElementById("layer-change");

        layerToggle.addEventListener("change", (event) => {
            console.log(event);
            map.setStyle("mapbox://styles/mapbox/" + event.target.value);
        });
        mapData.map((ele) => {
            new mapboxgl.Marker({
                draggable: true,
                color: "#" + (Math.random().toString(16) + "21F77E").substring(2, 8),
            })
                .setLngLat([ele.lat, ele.lon])
                .addTo(map);
        });
        // add polygon on pune location..........................
        // map.addSource("mumbai", {
        //     type: "geojson",
        //     data: {
        //         type: "Feature",
        //         geometry: {
        //             type: "Polygon",
        //             coordinates: [
        //                 [
        //                     [
        //                         72.85171508789062,
        //                         19.08547999705885
        //                     ],
        //                     [
        //                         72.84347534179688,
        //                         19.02057711096681
        //                     ],
        //                     [
        //                         72.91351318359375,
        //                         19.073799352002716
        //                     ],
        //                     [
        //                         72.85171508789062,
        //                         19.08547999705885
        //                     ]
        //                 ],
        //             ],
        //         },
        //     },
        // });
        // map.addLayer({
        //     id: "mumbai",
        //     type: "fill",
        //     source: "mumbai", // reference the data source
        //     layout: {},
        //     paint: {
        //         "fill-color": " gray", // blue color fill
        //         "fill-opacity": 1,
        //     },
        // });
    }
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

    //draw tool

    var Draw = new MapboxDraw();
    map.addControl(Draw, 'top-right');

    map.on('mousemove', (e) => {
        document.getElementById('info').innerHTML =
            // `e.point` is the x, y coordinates of the `mousemove` event
            // relative to the top-left corner of the map.
            JSON.stringify(e.point) +
            '<br />' +
            // `e.lngLat` is the longitude, latitude geographical position of the event.
            JSON.stringify(e.lngLat.wrap());
    });

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

.pre {
    z-index: 1;
    top: 0px;
    left: 600px;
    position: relative;
    width: 30%;
    height: 9%;
    background-color: white;
}
</style>
  