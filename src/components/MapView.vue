<template>
  <section>
     <Map @update-point="updatePoint"/>

     <div class="nav__desktop hidden-sm-and-down">
        <!-- Navigation -->
        <v-tabs
          v-model="tab"        
          background-color="#EF7C00"
          grow
          dark
          height="50px">
            <v-tab
              v-for="item in items"
              :key="item.tab"
            >
              {{ item.tab }}
            </v-tab>
        </v-tabs>

        <!-- Information -->
        <v-tabs-items v-model="tab" class="my-6">
          <v-tab-item
            transition="fade-transition">
            <v-card class="content pa-4" flat>
              <h1 class="text-body-1 mb-2">TAB1</h1>
              <p class="text-body-2 mb-0">Click the lines to see more information</p>
            </v-card>            
          </v-tab-item>

          <v-tab-item
            transition="fade-transition">
            <v-card class="content pa-4" flat v-if="!selectedPoint.Name">
              <h1 class="text-body-1 mb-2">TAB2</h1>
              <p class="text-body-2 mb-0">Click the circles to see more information</p>
            </v-card>
            <v-card class="content pa-4" flat v-if="selectedPoint.Name">
              <p class="text-body-1 mb-2">This is a selectedPoint from the map</p>
              <p class="text-h4 mb-4">{{ selectedPoint.Name  }}</p>
            </v-card>
          </v-tab-item>   
        </v-tabs-items>
     </div>

    <div class="nav__mobile hidden-md-and-up">
      <!-- Content to pull up -->
      <div class="bottom-nav__content-wrapper" :class="{ 'drawerState': showDrawer }">
        <div class="bottom-nav__content">
          <v-card class="content pa-4" flat>
            <h1 class="text-body-1 mb-2">TAB1</h1>
            <p class="text-body-2 mb-0">Click the lines to see more information</p>
          </v-card>
        </div>           
          <!-- <v-btn
            text
            :elevation="0"
            class="bottom-nav__content-button ma-0 pa-0">
            <v-icon color="#E9E2DA" style="font-size: 18px">mdi-close</v-icon>
          </v-btn>       -->
          <!-- <Legend v-show="selectedButton === 'legend'" />
          <LayerSwitchMobile  
            v-show="selectedButton === 'layers'"/>
          <InfoDrawerMobile  
            v-show="selectedButton === 'info'" />         -->
        
      </div>

      <!-- Nav bar -->
      <v-bottom-navigation
        color="#e9e2da"
        height="50px"
        background-color="transparent"
        grow
        class="elevation-0">

        <v-btn :ripple="false" height="auto" id="projects-mobile" @click="toggleDrawer('projects')">
          <span class="text-uppercase">
            <v-icon color="#E9E2DA">mdi-information-outline</v-icon>
            TAB1
          </span>          
        </v-btn>

        <v-btn :ripple="false" height="auto" id="networks-mobile" @click="toggleDrawer('networks')">
          <span class="text-uppercase">
            <v-icon color="#E9E2DA">mdi-information-outline</v-icon>
            TAB2
          </span>
        </v-btn>
      </v-bottom-navigation>
    </div>
  </section>

</template>

<script>
import Map from '@/components/viz/Map';

export default {
  name: 'MapView',
  components: {
    Map,
  },
  data() {
    return {
      tab: null,
      items: [
        { tab: 'TAB1', content: 'Tab 1 Content' },
        { tab: 'TAB2', content: 'Tab 2 Content' },
      ], 
      selectedPoint: {
        Name: '',
        Country: ''
      },
      showDrawer: false
    };
  },
  methods: {
    updatePoint(e) {
      this.selectedPoint = e;
    },
    toggleDrawer(selectedButton) {
      console.log(selectedButton);
      this.showDrawer = true;
    }
  },
}
</script>

<style scoped lang="scss">
section {
  position: relative;
}

.nav__desktop {
  width: 20vw; 
  position: absolute;
  z-index: 2;
  top: 32px;
  left: 32px;
}

.content {
  background-color: navy;
  max-height: 80vh;
}

.nav__mobile {
  position: absolute;
  bottom: 32px;
  width: 90vw;
  margin-left: -45vw;
  left: 50%;
}

// css for the bottom tabs
.v-bottom-navigation {
  z-index: 10;

  .v-btn {
    background-color: transparent !important;
    .v-icon, span {
      color: #EF7C00 !important;
    }
    border: 1px solid #EF7C00 !important;  
  }

  .v-btn:first-child {
    border-right: none;
  }


  .v-btn--active {
    background-color: #EF7C00 !important;
    .v-icon, span {
      color: white !important;
    }
  }  
}

.bottom-nav__content-wrapper{
  // overflow: hidden;  
  max-height: 100%;
	z-index: 2;
  background-color: transparent;
  position: absolute;
  bottom: 55px;
  width: 100%;
  pointer-events: none;
}

.bottom-nav__content {
  height: 0;
	// @include all-transition(0.5s);
  background-color: rgba(71, 56, 92, 0.9);
  position: relative;

  .bottom-nav__content-button  {
    position: absolute;
    top: 4px;
    right: -8px;
  }
}

.drawerState {
  // transform: translate3d(0, 0, 0);
	height: auto;
  // overflow: scroll;
  // @include all-transition(0.5s);

  animation-timing-function: 1.5s ease-in;
  pointer-events: auto;
}
</style>