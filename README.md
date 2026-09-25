## Basic mapicgc-gl-js viewer Svelte

* Basic mapicgc-gl-js viewer
* Svelte 5 + Vite 8

### Demo

* https://openicgc.github.io/basic-mapicgc-gl-js-viewer-svelte/dist/index.html

### Mapicgc-gl-js documentation

* https://openicgc.github.io/mapicgc-doc/

### Install

```bash
git clone https://github.com/OpenICGC/basic-mapicgc-gl-js-viewer-svelte.git
cd basic-mapicgc-gl-js-viewer-svelte
npm install
npm run dev
```

### Production build

```bash
npm run build:prod
npm run preview
```

### Example

```svelte
<script>
  import { onMount, onDestroy } from "svelte";
  import { Map, Config } from "mapicgc-gl-js";
  import "../node_modules/mapicgc-gl-js/dist/mapicgc-gl.css";

  let map;
  let mapContainer = $state();

  onMount(async () => {
    const data = await Config.getConfigICGC();

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
```

This example matches the current implementation in the Svelte component and uses the terrain source defined in the app.