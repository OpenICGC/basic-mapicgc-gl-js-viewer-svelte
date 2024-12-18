<script>
  import { onMount, onDestroy } from "svelte";
  import { Map, Config } from "mapicgc-gl-js";
  import "../node_modules/mapicgc-gl-js/dist/mapicgc-gl.css";

  let map;
  let mapContainer = $state(); 

  onMount(async () => {
    const data = await Config.getConfigICGC();
    console.info("què és això", data);
    map = new Map({
      container: mapContainer,
      style: data.Styles.TOPO,
      center: [1.808, 41.618],
      zoom: 10,
      maxZoom: 19,
      hash: true,
      pitch: 0,
    });

    map.on("load", () => {
      map.addGeocoderICGC();
      map.addGeolocateControl(
        {
          positionOptions: {
            enableHighAccuracy: true,
          },
          trackUserLocation: true,
        },
        "bottom-right"
      );
      map.addExportControl({}, "top-right");
      map.addFullscreenControl({}, "top-right");
      map.addTerrainICGC(data.Terrains.WORLD30M, "bottom-right");
    });
  });

  onDestroy(() => {
    if (map) {
      map.remove(); 
    }
  });
</script>

<div class="map-container">
  <div class="map" bind:this={mapContainer}></div>
</div>

<style>
  .map-container {
    position: relative;
    top:10vh;
    left:10vw;
    width: calc(
      90vw - 10vw
    ); 
    height: calc(
      90vh - 10vh
    );  
  }

  .map {
    position: absolute;
    width: 100%;
    height: 100%;
  }
</style>
