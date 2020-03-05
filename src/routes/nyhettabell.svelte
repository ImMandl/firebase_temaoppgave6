<script>
  import { onMount } from "svelte";
  import Artikler from "../components/Artikler.svelte";

  let gif = "Spinner-1s-200px.gif";

  let db; //ref til firestore
  let artikkelregister; // ref til collection i firestore
  let artikler = [];

  onMount(() => {
    db = firebase.firestore();
    artikkelregister = db.collection("artikkelregister");

    // viser frem artiklene
    artikkelregister.onSnapshot(snap => {
      artikler = snap.docs;
    });
  });
</script>

<style>
  table {
    width: 100%;
    border-collapse: collapse;
  }

  th {
    padding: 1rem 1.2rem;
    text-align: left;
  }

  thead {
    border-bottom: 2px solid #eee;
  }

  .middle {
    text-align: center;
  }

  .center {
    width: 100%;
  }

  .center img {
    display: block;
    margin: 0 auto;
  }
</style>

<svelte:head>
  <title>Nyheter</title>
</svelte:head>

<h1>Nyhetstabell</h1>

<table>
  <thead>
    <tr>
      <th>Bilde</th>
      <th>Tittel</th>
      <th>Kategori</th>
      <th>Laget</th>
      <th class="middle">Actions</th>
    </tr>
  </thead>
  <tbody>
    {#each artikler as artikkel}
      <Artikler data={artikkel.data()} id={artikkel.id} />
    {:else}
      <div class="center">
        <img src={gif} alt="loading gif" />
      </div>
    {/each}
  </tbody>
</table>
