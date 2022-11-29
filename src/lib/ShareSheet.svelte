<script>
  export let challenges;

  let color = ["blue", "red", "orange"];
</script>

<div class="challengeCard">
  {#each challenges as challenge, index}
    <h1 class={color[index]}>{challenge.title}</h1>
  {/each}
</div>

<style>
  .blue {
    border-bottom: 3px solid rgb(117, 117, 255);
  }

  .red {
    border-bottom: 3px solid rgb(246, 113, 113);
  }

  .orange {
    border-bottom: 3px solid rgb(251, 189, 103);
  }
</style>
