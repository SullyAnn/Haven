 <template>
    <div style="height: 600px; width: 100%">
        <div style="height: 200px; overflow: auto;">
        <p>First marker is placed at {{ withPopup.lat }}, {{ withPopup.lng }}</p>
        <p>Center is at {{ currentCenter }} and the zoom is: {{ currentZoom }}</p>
        <button @click="showLongText">
            Toggle long popup
        </button>
        <button @click="showMap = !showMap">
            Toggle map
        </button>
        </div>
        <l-map
        v-if="showMap"
        :ref="map"
        v-model:zoom="zoom" 
        :center="center"
        :options="mapOptions"
        style="height: 100%"
        @update:center="centerUpdate"
        @update:zoom="zoomUpdate"
        >
        <l-tile-layer
            :url="url"
            :attribution="attribution"
        />
        <l-marker :lat-lng="withPopup">
            <l-popup>
            <div @click="innerClick">
                I am a popup
                <p v-show="showParagraph">
                Lorem ipsum dolor sit amet, consectetur adipiscing elit. Quisque
                sed pretium nisl, ut sagittis sapien. Sed vel sollicitudin nisi.
                Donec finibus semper metus id malesuada.
                </p>
            </div>
            </l-popup>
        </l-marker>
        <l-marker :lat-lng="withTooltip">
            <l-tooltip :options="{ permanent: true, interactive: true }">
            <div @click="innerClick">
                I am a tooltip
                <p v-show="showParagraph">
                Lorem ipsum dolor sit amet, consectetur adipiscing elit. Quisque
                sed pretium nisl, ut sagittis sapien. Sed vel sollicitudin nisi.
                Donec finibus semper metus id malesuada.
                </p>
            </div>
            </l-tooltip>
        </l-marker>
        </l-map>
    </div>
</template>

<script>
import { LMap, LTileLayer, LMarker, LPopup, LTooltip } from "@vue-leaflet/vue-leaflet";

export default {
  name: "MapComponent",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
    LTooltip
  },
  data() {
    return {
      zoom: 13,
      center: {lat: "48.8333", lng: "2.6167"},
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      withPopup: {lat: "48.8333", lng: "2.6167"},
      withTooltip: {lat: "48.8333", lng: "2.6167"},
      currentZoom: 11.5,
      currentCenter: {lat: "48.8333", lng: "2.6167"},
      showParagraph: false,
      mapOptions: {
        zoomSnap: 0.5
      },
      showMap: true
    };
  },
  methods: {
    zoomUpdate(zoom) {
      this.currentZoom = zoom;
    },
    centerUpdate(center) {
      this.currentCenter = center;
    },
    showLongText() {
      this.showParagraph = !this.showParagraph;
    },
    innerClick() {
      alert("Click!");
    }
  }
};
</script>