<script>
  import { onMount } from "svelte";

  let db; //ref til firestore
  let artikkelregister; // ref til collection i firestore
  let slettArtikkel; // skal slette en artikkel fra databasen
  let redigerArtikkel; // skal redigere en artikkel fra databasen

  let artikkel;
  let toggleEditMode; // toggle edit mode on and off
  let editmode = false; // edit starter som false
  let oppdaterArtikkel; // oppdater en artikkel med ny/endret info

  export let data = {};
  export let id = "";

  onMount(() => {
    db = firebase.firestore();
    artikkelregister = db.collection("artikkelregister");

    // filtrerer artiklene etter tittel
    artikkelregister.orderBy("tittel", "desc");

    // rediger artikkel
    toggleEditMode = () => {
      editmode = !editmode;
    };

    // sletter artikler
    slettArtikkel = () => {
      artikkel = artikkelregister.doc(id);
      artikkel.delete();
    };

    // Oppdaterer artikkel i editmode
    oppdaterArtikkel = () => {
      artikkel = artikkelregister.doc(id);
      artikkel.update({
        tittel: data.tittel,
        tekst: data.tekst
      });
    };
  });
</script>

<style>
  td {
    padding: 1rem 1.2rem;
    text-align: left;
  }

  tr:nth-child(even) {
    background-color: #eee;
  }

  .delete {
    background-color: tomato;
    color: #fff;
    cursor: pointer;
  }

  .delete:hover {
    background-color: rgb(202, 61, 36);
  }

  .edit {
    background-color: teal;
    color: #fff;
    cursor: pointer;
  }

  .edit:hover {
    background-color: rgb(2, 92, 92);
  }

  .middle {
    text-align: center;
  }

  img {
    height: 50px;
    width: 50px;
    object-fit: cover;
  }

  .form {
    display: flex;
    flex-direction: column;
  }

  .tid {
    font-size: 10px;
  }
</style>

{#if editmode}
  <tr>
    <td>
      <img src={data.url} alt="artikkel bilde" />
    </td>
    <div class="form">
      <td>
        <input
          bind:value={data.tittel}
          placeholder={data.tittel}
          on:input={oppdaterArtikkel} />
      </td>
      <td>
        <textarea
          bind:value={data.tekst}
          placeholder={data.tekst}
          on:input={oppdaterArtikkel}
          cols="60"
          rows="10" />
      </td>
    </div>
    <td>{data.kategori}</td>
    <td />
    <td class="middle">
      <button class="edit" on:click={toggleEditMode}>
        <i class="fas fa-save" />
      </button>
      <button class="delete" on:click={slettArtikkel}>
        <i class="fas fa-trash" />
      </button>
    </td>
  </tr>
{:else}
  <tr>
    <td>
      <img src={data.url} alt="artikkel bilde" />
    </td>
    <td>{data.tittel}</td>
    <td>{data.kategori}</td>
    <td class="tid">{data.tid}</td>
    <td class="middle">
      <button class="edit" on:click={toggleEditMode}>
        <i class="fas fa-edit" />
      </button>
      <button class="delete" on:click={slettArtikkel}>
        <i class="fas fa-trash" />
      </button>
    </td>
  </tr>
{/if}
