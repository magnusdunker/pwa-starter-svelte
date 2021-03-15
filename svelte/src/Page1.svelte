<script lang="ts">
  import { onMount } from "svelte";
  import "smelte/src/tailwind.css";
  import { Select } from "svelte-materialify";
  import axios from "axios";
  export let params;
  let name = "[All]";
  let lat;
  let lng;
  let geoId;
  let list = [];
  let dist = 20000;
  onMount(() => {
    geoId = navigator.geolocation.getCurrentPosition(watch, (error) => {
      console.error(error);
    });
    return () => navigator.geolocation.clearWatch(geoId);
  });
  async function watch(position: GeolocationPosition) {
    lat = position.coords.latitude;
    lng = position.coords.longitude;
    list = (await axios.get(`${URL}/${lng}/${lat}/${dist}`)).data;
  }
  function handleChange(e: any) {
    name = e.target.value;
  }
  function handleClick(l) {
    console.log(l);
    window.open(`https://www.google.com/maps/search/?api=1&query=${l.coords.latitude},${l.coords.longitude}`);
  }
  function getDistanceFromLatLonInKm(lat1, lon1, lat2, lon2) {
    var R = 6371; // Radius of the earth in km
    var dLat = deg2rad(lat2 - lat1); // deg2rad below
    var dLon = deg2rad(lon2 - lon1);
    var a =
      Math.sin(dLat / 2) * Math.sin(dLat / 2) + Math.cos(deg2rad(lat1)) * Math.cos(deg2rad(lat2)) * Math.sin(dLon / 2) * Math.sin(dLon / 2);
    var c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
    var d = R * c; // Distance in km
    return d;
  }

  function deg2rad(deg) {
    return deg * (Math.PI / 180);
  }
  const items = [
    { name: "[All]", value: "[All]" },
    { name: "Hindbær", value: "Hindbær" },
    { name: "Enebær", value: "Enebær" },
    { name: "Skovæble", value: "Skovæble" },
    { name: "Porse", value: "Porse" },
    { name: "Tormentil", value: "Tormentil" },
  ];
</script>

<main class="main">
  <Select on:change={(e) => console.log(e)} solo {items} value={name} placeholder="Sanke" />
  {#each list as l, i}
    <div
      class="container"
      on:click={(e) => {
        handleClick(l);
      }}
    >
      <div class="title">
        {l.name}
        {getDistanceFromLatLonInKm(lat, lng, l.coords.latitude, l.coords.longitude).toFixed(1)}&nbsp; km
      </div>
      <div class="container-row">
        <div class="timestamp">
          {new Date(l.timeStamp).getDate()}-{new Date(l.timeStamp).getMonth() + 1}-{new Date(l.timeStamp).getFullYear()}
        </div>
        <div class="timestamp">{l.coords.accuracy}&nbsp;m</div>
      </div>
    </div>
  {/each}
</main>

<!-- <TextField value={name} on:change={handleChange}>Title</TextField> -->
<style>
  .main {
    width: 100vw;
  }
  .timestamp {
    text-align: left;
  }
  .container {
    display: flex;
    flex-direction: column;
    border-bottom: 1px solid #ccc !important;
    padding: 8px;
  }
  .container-row {
    display: flex;
    flex-direction: row;
    padding: 8px;
    justify-content: space-between;
  }
  .title {
    font-weight: bold;
    font-size: 18px;
    margin-top: 6px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
</style>
