<template>
  <div id="map" class="map"/>
</template>

<script>
import _Mapbox from '@/plugins/mapbox';

export default {
  name: 'Map',
  data() {
    return {
      map: null,  
    };
  },
  computed: {
    defaultOptions() {
      if (this.$vuetify.breakpoint.smAndDown) {
        return {
          container: 'map',
          style: 'mapbox://styles/mapbox/light-v10',
          center: [105.364, 12.554],
          zoom: 2.8,          
        };
      }
      return {
        container: 'map',
        style: 'mapbox://styles/mapbox/light-v10',
        center: [70.597, 8.486],
        zoom: 2.01,
      };
    },
  },
  mounted() {
    // listen to window size
    window.addEventListener('load', () => {
      let vh = window.innerHeight * 0.01;
      document.getElementById('map').style.setProperty('--vh', `${vh}px`);
    }); 

    // init Mapbox
    this.map = _Mapbox.createMap(this.defaultOptions);

    // basic map interaction setting 
    this.map.addControl(
      _Mapbox.createControl({showCompass: false}), 
      this.$vuetify.breakpoint.smAndDown ? 'top-right' : 'top-right');
    this.map.touchZoomRotate.disableRotation();
    this.map.touchPitch.disable();

    this.map.on('load', () => {
      this.addPointsLayer();
    });

    this.handleInteractivity();
  },
  methods: {  
    addPointsLayer() {
      this.map.addSource('points', {
        type: 'geojson',
        data: `data/points.json`,
      });     
      this.map.addLayer({
        id: 'project',
        type: 'circle',
        source: 'points',
        paint: {
          'circle-radius': [
            'interpolate', 
            ['linear'], ['zoom'], 
            1, 5,
            5, 8, 
            10, 12, 
          ],              
          'circle-stroke-color': 'white',
          'circle-stroke-width': 1,
          'circle-color': '#605675',
        },
      });
    },
    handleInteractivity() {
      // const layers = ['project'];
      // layers.forEach(layer => {
      //   this.map.on('mouseenter', layer, this.setTooltip);
      //   this.map.on('mouseleave', layer, this.exitTooltip);        
      // });

      this.map.on('click', 'project', e => {
        // map action
        const coord = e.features[0].geometry.coordinates;
        // const currentZoom = this.map.getZoom();
        this.map.flyTo({ center: [coord[0], coord[1]], zoom: 7});

        // data action
        this.$emit('update-point', e.features[0].properties);      
      });
    }
  },
};
</script>

<style lang="scss" scoped>
@import '~mapbox-gl/dist/mapbox-gl.css';

#map {
  width: 100%;
  height: calc(var(--vh, 1vh) * 100);
  position: relative;
  
  @media #{map-get($display-breakpoints, 'sm-and-down')} {
    height: calc(var(--vh, 1vh) * 100);
  }    
}


// ::v-deep .custom-tooltip {
//   .mapboxgl-popup-content {
//     border-radius: 3px;
//     padding: 16px;
//     background-color: rgba(233, 226, 218, 0.9);
//     box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
//     border: 2px solid #6d58ad;  
//   }
//   .mapboxgl-popup-tip {
//     display: none !important;
//   }
// }
</style>


  