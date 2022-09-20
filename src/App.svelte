<script lang="ts">
  import { onMount } from 'svelte';
  import Dropdown from './lib/Dropdown.svelte';
  import Modal from './lib/Modal.svelte';
  import chuckNorrisLogo from './assets/chuck-norris.png';

  interface Joke {
    categories: Array<string>;
    created_at: string;
    icon_url: string;
    id: string;
    updated_at: string;
    url: string;
    value: string;
  }

  let selectedCategory: string;
  let categories: Array<string> = [];
  let joke: Joke;
  let modal: Modal;

  onMount(async () => {
    void fetchCategories();
  });

  async function fetchCategories() {
    const url = 'https://api.chucknorris.io/jokes/categories';

    try {
      const response = await fetch(url);
      categories = await response.json();
      selectedCategory = categories[0];
    } catch (e) {
      console.error('Error fetching categories:', e);
    }
  }

  async function fetchJoke() {
    const url = `https://api.chucknorris.io/jokes/random?category=${selectedCategory}`;

    try {
      const response = await fetch(url);
      joke = await response.json();
      modal.open();
    } catch (e) {
      console.error('Error fetching joke:', e);
    }
  }
</script>

<main>
  <div class="dropdown">
    <Dropdown {categories} bind:value={selectedCategory} />
  </div>

  <Modal bind:this={modal}>
    <button on:click={fetchJoke} slot="trigger" type="button">
      Joke It Up
    </button>

    <div slot="content">
      <img class="img" src={chuckNorrisLogo} alt="Chuck Norris" />
      <p>{joke.value}</p>
    </div>
  </Modal>
</main>

<style>
  .dropdown {
    margin-bottom: 20px;
  }

  .img {
    background-color: yellow;
    width: 100px;
    border-radius: 50%;
    border: 2px solid black;
  }
</style>
