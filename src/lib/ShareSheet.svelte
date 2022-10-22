<script>
  export let challenges;
</script>

<div class="challengeCard">
  {#each challenges as challenge}
    <h1>{challenge.title}</h1>
  {/each}
</div>

<style>
  .challengeCard{
    
  }
</style>