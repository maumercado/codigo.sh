<script>
  import Header from '../components/Header.svelte';
  export let segment;
</script>

<style>
  .layout {
    display: flex;
    flex-direction: column;
    min-height: 100%;
    min-height: 100vh;
  }
  main {
    flex: 1;
    position: relative;
    margin: 0 auto;
    max-width: 1400px;
    background-color: white;
    padding: 1em 1.5em;
    box-sizing: border-box;
    width: 80%;
    display: flex;
    flex-direction: column;
  }

   @media (max-width: 1020px) {
    main {
      width: 90%;
    }
  }
  @media (max-width: 800px) {
    main {
      width: 100%;
    }

  }

  footer {
    color: #AAA;
    font-size: 1em;
    font-family: Rubik, sans-serif;
    margin: 1em auto;
    max-width: 1400px;
    padding: 1em 2em;
    text-align: center;
    width: 100%;
  }
</style>

<div class="layout">
  <Header {segment}/>

  <main>
    <slot></slot>
  </main>

  <footer>
    <span>
      &copy; {new Date().getFullYear()} Codigo.
      Powered by <a href="https://sapper.svelte.dev" target="_blank">Sapper</a>.
    </span>
  </footer>
</div>