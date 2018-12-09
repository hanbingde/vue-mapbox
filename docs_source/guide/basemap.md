# Base map

## Adding map component

For using maps with Mapbox GL JS you need a map style. See details in Mapbox GL JS [documentation](https://mapbox.com/mapbox-gl-js/style-spec).  
For using Mapbox-hosted maps, you need to set access_token. See details in Mapbox [documentation](https://mapbox.com/help/define-access-token/).  
If you using self-hosting maps on your own server you can omit this parameter.

```vue
<template>
<mgl-map
    :accessToken="accessToken"
    :mapStyle.sync="mapStyle"
/>
</template>

<script>
import { MglMap } from 'vue-mapbox'

export default {
  components: {
    MglMap
  },
  data() {
    return {
      accessToken: ACCESS_TOKEN, // your access token. Needed if you using Mapbox maps
      mapStyle: MAP_STYLE, // your map style
    }
  }
}
</script>
```

### Interact with map properties as GlMap props

You can control map parameters like zoom, bearing, pitch etc. by changing props.
If you set .sync modifier ([Vue docs](https://vuejs.org/v2/guide/components.html#sync-Modifier)) to prop, it will updates when you use operations that takes time to proceed. For example, if you use flyTo method, props zoom, center, bearing, pitch will be updated when animation ends.
<!-- See example with flyTo:
example with flyTo -->
Full list of props see in [API docs](api/glmap.md#props), note field 'Synced' in description

## Map actions

Asynchronous map methods exposed at GlMap component in `actions` property. They returns `Promise`, that resolves when action completed.
Promise resolves with map properties that has been changed by used action.
For example:

```vue{2}
<script>
export deafult {
  name: 'App',

  methods: {
    aync onMapLoad(event) {
      // Here we cathing 'load' map event
      const asyncActions = event.component.actions

      const newParams = await asyncActions.flyTo({
        center: [30, 30],
        zoom: 9,
        speed: 1
      })
      console.log(newParams)
      /* => { 
              center: [30, 30],
              zoom: 9,
              bearing: 9,
              pitch: 7
            }
      */
    }
  }
}
</script>
```

See full list of actions on [API](/api/glmap.md#actions) page.

### Method `actions.stop()`

Method `.stop()` just stops all animations on map, updates props with new positions and return Promise with map parameters in the moment when stop() called.  