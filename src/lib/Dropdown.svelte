<script lang="ts">
  export let value: string;
  export let categories: Array<string>;
</script>

<label for="categories">Select a category:</label>
<select name="categories" id="categories" bind:value>
  {#each categories as category}
    <option value={category}>{category}</option>
  {/each}
</select>
