## basic mapicgc-gl-js viewer sevelt

  * Basic mapicgc-gl-js viewer
  * Svelte + Vite


### To install

```bash
git clone https://github.com/OpenICGC/basic-mapicgc-gl-js-viewer-svelte.git

cd /basic-mapicgc-gl-js-viewer-svelte

npm install
npm run build
npm run dev

```

```javascript
  import { onMount, onDestroy } from "svelte";
  import { Map, Config } from "mapicgc-gl-js";
  import "../node_modules/mapicgc-gl-js/dist/mapicgc-gl.css";

  let map;
  let mapContainer;

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
  });

```