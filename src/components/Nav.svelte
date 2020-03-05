<script>
  export let segment;

  let logo = "bjorneposten-logo.svg";

  import { onMount } from "svelte";
  import { authState } from "rxfire/auth";
  let db; // ref til firestore
  let auth; // authentication
  let googleProvider; // Google innlogging
  let artikkelregister; // ref til collection i firestore
  let login; // logg inn
  let logout; // logg ut
  let showLogout = false; // viser ikke logout automatisk
  let toggleLogout; // viser logout etter trykt profilbilde
  let user; // bruker
  let unsubscribe;

  onMount(() => {
    db = firebase.firestore();
    auth = firebase.auth();
    googleProvider = new firebase.auth.GoogleAuthProvider();
    artikkelregister = db.collection("artikkelregister");

    login = () => {
      auth.signInWithPopup(googleProvider);
    };

    logout = () => {
      auth.signOut();
    };

    toggleLogout = () => {
      showLogout = !showLogout;
    };

    unsubscribe = authState(auth).subscribe(u => (user = u));
  });
</script>

<style>
  header {
    position: fixed;
    top: 0;
    left: 0;
    height: 100%;
    width: 200px;
    z-index: 99;
    box-shadow: 2px 1px 4px rgba(33, 33, 33, 0.4);
    background-color: #fff;
  }

  nav {
    display: grid;
    margin-top: 0.5rem;
  }

  [aria-current] {
    position: relative;
    display: inline-block;
    background-color: #f9f9f9;
  }

  [aria-current]::after {
    position: absolute;
    content: "";
    height: calc(100%);
    width: 4px;
    background-color: tomato;
    display: block;
    top: 0;
    left: 0;
  }

  a {
    text-decoration: none;
    padding: 0.8em 0.5em 0.8em 1em;
    display: block;
  }

  a:hover {
    text-decoration: underline;
  }

  .logo {
    width: 140px;
    padding: 1.6em 0 1em 0.6em;
  }

  /* Topbar og profil */
  section {
    position: fixed;
    top: 0;
    left: 0;
    z-index: -10;
    height: 80px;
    width: 100%;
    background-color: tomato;
    box-shadow: 2px 1px 4px rgba(33, 33, 33, 0.4);
  }

  .profil-container {
    float: right;
    margin: 1rem;
    cursor: pointer;
  }

  .profil {
    display: flex;
    flex-direction: row;
  }

  .profil p {
    color: white;
  }

  .logout-btn {
    background-color: tomato;
    color: #fff;
    border: none;
    float: right;
    padding: 0.8rem 1rem;
  }

  .logout-btn:hover {
    color: #333;
  }

  .profilbilde {
    height: 50px;
    width: 50px;
    border-radius: 50%;
    cursor: pointer;
  }

  .name {
    color: #333;
    margin-right: 0.5rem;
  }

  /* before logged in */
  .cover {
    min-width: 100vw;
    width: 100%;
    min-height: 100vh;
    height: 100%;
    background-color: tomato;
    z-index: 998;
    position: relative;
    display: grid;
  }

  .login-btn {
    z-index: 999;
    background-color: white;
    border: none;
    justify-self: center;
    align-self: center;
  }

  .login-btn:hover {
    color: tomato;
  }
</style>

<header>
  {#if user}
    <img class="logo" src={logo} alt="Bjørneposten logo" />
    <nav>
      <a aria-current={segment === undefined ? 'page' : undefined} href=".">
        Dashboard
      </a>
      <a
        aria-current={segment === 'nyhettabell' ? 'page' : undefined}
        href="nyhettabell">
        Nyhetstabell
      </a>
      <a
        aria-current={segment === 'lagartikkel' ? 'page' : undefined}
        href="lagartikkel">
        Lag ny artikkel
      </a>
    </nav>
    <section>
      <div class="profil-container">
        <div class="profil" on:click={toggleLogout}>
          <p class="name">{user.displayName}</p>
          <img class="profilbilde" src={user.photoURL} alt="profilbilde" />
        </div>
        {#if showLogout}
          <button class="logout-btn" on:click={logout}>
            Logg ut
            <i class="fas fa-sign-out-alt" />
          </button>
        {/if}
      </div>
    </section>
  {:else}
    <div class="cover">
      <button class="login-btn" on:click={login}>Logg inn</button>
    </div>
  {/if}
</header>
