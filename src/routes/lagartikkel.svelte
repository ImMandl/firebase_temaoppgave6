<script>
  import { onMount } from "svelte";

  let db; // ref til firestore
  let storage; // ref til storage i firestore
  let artikkelregister; // ref til collection i firestore

  let tittel = ""; // tittelen til artikkel
  let tekst = ""; // brødteksten til artikkel
  let url = ""; // url til bilde fra storage
  let kategori = "nyhet"; // setter kategori for artikkel. Starter på nyhet
  let files = [];
  $: file = files[0];
  let tid = firebase.firestore.FieldValue.serverTimestamp();

  let lastOppBilde; // laster bildet opp til storage

  onMount(() => {
    db = firebase.firestore();
    storage = firebase.storage();
    artikkelregister = db.collection("artikkelregister");

    // laster opp bildet
    lastOppBilde = async () => {
      const sti = storage.ref().child("bjorneposten_bilder/" + file.name);

      const opplasting = await sti.put(file);
      const urlen = await opplasting.ref.getDownloadURL();
      url = urlen;
      registrerArtikkel();
    };
  });

  // laster opp artikkel informasjon
  const registrerArtikkel = () => {
    artikkelregister.doc().set({ url, tittel, tekst, kategori, tid });

    url = "";
    tittel = "";
    tekst = "";
    kategori = "";
    tid = "";
  };
</script>

<style>
  form {
    margin-top: 2em;
  }

  input {
    margin-right: 0.8em;
  }

  form {
    display: flex;
    flex-direction: column;
  }

  form div {
    margin-bottom: 1rem;
  }

  /*  input[type="radio"] {
    margin: 0;
  }
 */
  label {
    margin-right: 1rem;
  }

  /* radio button style */
  /* The container */
  .container {
    position: relative;
    padding-left: 25px;
    margin-bottom: 12px;
    cursor: pointer;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
  }

  /* Hide the browser's default radio button */
  .container input {
    position: absolute;
    opacity: 0;
    cursor: pointer;
  }

  /* Create a custom radio button */
  .checkmark {
    position: absolute;
    top: 0;
    left: 0;
    height: 20px;
    width: 20px;
    background-color: #eee;
    border-radius: 50%;
  }

  /* On mouse-over, add a grey background color */
  .container:hover input ~ .checkmark {
    background-color: #ccc;
  }

  /* When the radio button is checked, add a blue background */
  .container input:checked ~ .checkmark {
    background-color: tomato;
  }

  /* Create the indicator (the dot/circle - hidden when not checked) */
  .checkmark:after {
    content: "";
    position: absolute;
    display: none;
  }

  /* Show the indicator (dot/circle) when checked */
  .container input:checked ~ .checkmark:after {
    display: block;
  }

  /* Style the indicator (dot/circle) */
  .container .checkmark:after {
    top: 6px;
    left: 6px;
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: white;
  }
  /* radio button style end */
</style>

<svelte:head>
  <title>Registrer nye artikkel</title>
</svelte:head>

<h1>Registrer en ny artikkel</h1>

<form on:submit|preventDefault={lastOppBilde}>
  <div>
    <input bind:value={tittel} placeholder="Tittel" required />
    <input type="file" bind:files required />
  </div>
  <div>
    <textarea
      bind:value={tekst}
      placeholder="Brødtekst"
      cols="60"
      rows="10"
      required />
  </div>
  <div>
    <label class="container">
      Nyhet
      <input type="radio" name="radio" bind:group={kategori} value={'nyhet'} />
      <span class="checkmark" />
    </label>
    <label class="container">
      Sport
      <input type="radio" name="radio" bind:group={kategori} value={'sport'} />
      <span class="checkmark" />
    </label>
    <label class="container">
      Kultur
      <input type="radio" name="radio" bind:group={kategori} value={'kultur'} />
      <span class="checkmark" />
    </label>
  </div>
  <div>
    <button id="submit" type="submit">Lag artikkel</button>
  </div>
</form>
