<template>
    <div id="map"></div>
</template>

<script>
import 'mapbox-gl/dist/mapbox-gl.css'
import mapboxgl from 'mapbox-gl'

import { Deck } from '@deck.gl/core'
import { MapboxLayer } from '@deck.gl/mapbox'
import { ScenegraphLayer } from '@deck.gl/mesh-layers'
import { registerLoaders } from '@loaders.gl/core'
import { GLTFLoader } from '@loaders.gl/gltf'
// import { GLTFEnvironment } from '@luma.gl/experimental'
// import GL from '@luma.gl/constants'

// Register the proper loader for scenegraph.gltf
registerLoaders([GLTFLoader])

// Constants to load GLTF environment
// const GLTF_BASE_URL = 'luma.gl/examples/gltf/'

// const CUBE_FACE_TO_DIRECTION = {
//     [GL.TEXTURE_CUBE_MAP_POSITIVE_X]: 'right',
//     [GL.TEXTURE_CUBE_MAP_NEGATIVE_X]: 'left',
//     [GL.TEXTURE_CUBE_MAP_POSITIVE_Y]: 'top',
//     [GL.TEXTURE_CUBE_MAP_NEGATIVE_Y]: 'bottom',
//     [GL.TEXTURE_CUBE_MAP_POSITIVE_Z]: 'front',
//     [GL.TEXTURE_CUBE_MAP_NEGATIVE_Z]: 'back',
// }

// const IMAGE_BASED_LIGHTING_ENVIRONMENT = {
//     brdfLutUrl: `${GLTF_BASE_URL}/brdfLUT.png`,
//     getTexUrl: (type, dir, mipLevel) => `${GLTF_BASE_URL}/papermill/${type}/${type}_${CUBE_FACE_TO_DIRECTION[dir]}_${mipLevel}.jpg`,
// }

export default {
    mounted() {
        mapboxgl.accessToken = 'pk.eyJ1IjoiaWZ6bSIsImEiOiJjamswc3M1a2gwYWEwM3Zxa3ZsMjh3djkwIn0.h93-XtdRdtoC1KbVv_ZIow'

        const map = new mapboxgl.Map({
            container: 'map',
            style: 'mapbox://styles/mapbox/navigation-guidance-night-v4',
            center: [112.994, 28.035],
            zoom: 18,
            pitch: 50,
            bearing: -120,
        })

        const deck = new Deck({
            gl: map.painter.context.gl,
            layers: [
                new ScenegraphLayer({
                    id: 'scenegraph-layer',
                    data: [[112.994, 28.035]],
                    getPosition: f => f,
                    sizeScale: 2.8,
                    getOrientation: [0, 270, 90],
                    getTranslation: [0, 0, 30],
                    getScale: [1, 1, 1],
                    // _lighting: 'pbr',
                    // _imageBasedLightingEnvironment: ({ gl }) => new GLTFEnvironment(gl, IMAGE_BASED_LIGHTING_ENVIRONMENT),
                    // scenegraph: 'https://hujiulong.github.io/vue-3d-model/static/models/gltf/Duck/glTF/Duck.gltf',
                    scenegraph: 'shuri.glb',
                    // scenegraph: `${GLTF_BASE_URL}/DamagedHelmet.glb`,
                }),
            ],
        })

        map.on('load', () => {
            map.addLayer(
                new MapboxLayer({
                    id: 'scenegraph-layer',
                    deck,
                }),
            )
        })
    },
}
</script>

<style scoped>
#map {
    width: 100%;
    height: 700px;
}
</style>
