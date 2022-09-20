<script lang="ts">
  import { onMount } from 'svelte';
  import Dropdown from './lib/Dropdown.svelte';

  let value: string;
  let categories: Array<string> = [];

  onMount(async () => {
    void fetchCategories();
  });

  async function fetchCategories() {
    const url = 'https://api.chucknorris.io/jokes/categories';

    try {
      const response = await fetch(url);
      categories = await response.json();
      value = categories[0];
    } catch (e) {
      console.error('Error fetching categories:', e);
    }
  }
</script>

<main>
  <div>
    <Dropdown {categories} {value} />
  </div>
</main>

<style>
</style>
