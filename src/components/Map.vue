<template>
  <div id="map" class="my-2 shadow">
    <div id="mapid"></div>
  </div>
</template>

<script>
import L from "leaflet";
import icon from "leaflet/dist/images/marker-icon.png";
import iconShadow from "leaflet/dist/images/marker-shadow.png";

export default {
  name: "Map",
  props: {
        locations: Array
  },
  //   data: {},
  mounted() {
    let DefaultIcon = L.icon({
      iconUrl: icon,
      shadowUrl: iconShadow
    });

    L.Marker.prototype.options.icon = DefaultIcon;
    var mymap = L.map("mapid").setView([19.4326, -99.1332], 12);
    L.tileLayer(
      "https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token=pk.eyJ1IjoiZmFua3MiLCJhIjoiY2p3M3R6YWc1MGphajQ4bm0ydmUza2NkdCJ9.lThsFUgWmrCPc64lSlKIFg",
      {
        maxZoom: 18,
        id: "mapbox.light",
        accessToken: "your.mapbox.access.token"
      }
    ).addTo(mymap);
    // var marker = L.marker([19.4326, -99.1332]).addTo(mymap);
    console.log(this.locations)
    this.locations.forEach(l => {
        let marker = L.marker([l.Latitude, l.Longitude]).addTo(mymap);
        marker.bindPopup(`<b class="mb-0">${l["Store Name"]}</b> <br> ${l["Street Address"]}`).openPopup();
    });
  }
};
</script>

<style lang="scss" scoped>
#mapid {
  height: 400px;
}
</style>
