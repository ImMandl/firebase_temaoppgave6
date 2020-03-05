<script>
  import { onMount } from "svelte";

  let db; //ref til firestore
  let artikkelregister; // ref til collection i firestore
  let artikler = [];

  onMount(() => {
    db = firebase.firestore();
    artikkelregister = db.collection("artikkelregister");

    artikkelregister.onSnapshot(snap => {
      artikler = snap.docs;
    });
  });
</script>

<style>
  section {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(4em, 10em));
    gap: 2rem;
  }

  .card {
    border-radius: 8px;
    padding: 1rem;
    background: tomato;

    display: flex;
    flex-direction: column;
  }

  .card-numbers {
    display: flex;
    flex-direction: row;
    align-items: center;
  }

  .card h3 {
    font-weight: 500;
    color: white;
    margin: 0;
  }

  .card h4 {
    color: white;
    font-weight: 300;
  }

  i {
    font-size: 2rem;
    color: white;
    margin-right: 0.4rem;
  }
</style>

<svelte:head>
  <title>Dashboard</title>
</svelte:head>

<h1>Dashboard</h1>

<section>
  <div class="card">
    <h4>Artikler</h4>
    <div class="card-numbers">
      <i class="fas fa-chart-line" />
      <h3>{artikler.length}</h3>
    </div>
  </div>
  <div class="card">
    <h4>Nyheter</h4>
    <div class="card-numbers">
      <i class="fas fa-newspaper" />
      <!-- <h3>{artikkelregister.data('kategori').length}</h3> -->
    </div>
  </div>
  <div class="card">
    <h4>Sport</h4>
    <div class="card-numbers">
      <i class="fas fa-futbol" />
      <!-- <h3>{artikler.length}</h3> -->
    </div>
  </div>
  <div class="card">
    <h4>Kultur</h4>
    <div class="card-numbers">
      <i class="fas fa-landmark" />
      <!-- <h3>{artikler.length}</h3> -->
    </div>
  </div>
  <div class="card">
    <h4>Bilder</h4>
    <div class="card-numbers">
      <i class="fas fa-image" />
      <!-- <h3>{artikler.length}</h3> -->
    </div>
  </div>
</section>
