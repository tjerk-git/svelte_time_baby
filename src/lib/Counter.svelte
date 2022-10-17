<script>

   import Typewriter from 'svelte-typewriter'
   import { fade } from 'svelte/transition';

  let tasks = [
  { id: 1, title: 'Design a', status: false},
  { id: 2, title: 'Create a', status: false },
  { id: 3, title: 'Prototype a', status: false },
  { id: 4, title: 'Sketch a', status: false },
  ];

  let contexts = [
  { id: 1, title: 'Robot for', status: false},
  { id: 2, title: 'Playstation for', status: false },
  { id: 3, title: 'Website for', status: false },
  { id: 4, title: 'Mobile app for', status: false },
  ];

    let audiences = [
  { id: 1, title: 'Aliens', status: false},
  { id: 2, title: 'Your mother', status: false },
  { id: 3, title: 'For the elderly', status: false },
  { id: 4, title: 'Hamsters with long hair', status: false },
  ];

  let tasksNum = 1;
  let contextsNum = 1;
  let audiencesNum = 1;
  let tokens = 2;
  let taskNumLocked = false;
  let contextsNumLocked = false;
  let audiencesNumLocked = false;

  function spinTheWheel() {
    if (taskNumLocked === false) {
      tasksNum = Math.floor(Math.random() * tasks.length);
    }
    if (contextsNumLocked === false) {
      contextsNum = Math.floor(Math.random() * contexts.length);
    }

    if (audiencesNumLocked === false) {
       audiencesNum = Math.floor(Math.random() * audiences.length);
    }
  }

  /**
   * @param {number} id
   */
  function spendToken(id){

    if(tokens > 0){
      tokens = tokens -1;
    } else {
      return;
    }
    
    if(id === 1){
      taskNumLocked = true;
    }

    if(id === 2){
      contextsNumLocked = true;
    }

    if(id === 3){
      audiencesNumLocked = true;
    }

  }

</script>
<h1>Tokens:
	{#each [tokens] as c (c)}
	<strong in:fade>{c}</strong> 
	{/each}
</h1>

{#if taskNumLocked == false}
  <Typewriter mode="scramble" wordInterval: 1>
      <h1>{tasks[tasksNum].title}</h1> 
  </Typewriter>
  <button on:click={() => spendToken(1)}>Lock</button>
{:else}
    <h1>{tasks[tasksNum].title}</h1> 
{/if}

{#if contextsNumLocked == false}
  <Typewriter mode="scramble" wordInterval: 1>
      <h1>{contexts[contextsNum].title}</h1> 
  </Typewriter>
    <button on:click={() => spendToken(2)}>Lock</button>
{:else}
    <h1>{contexts[contextsNum].title}</h1> 
{/if}


{#if audiencesNumLocked == false}
  <Typewriter mode="scramble" wordInterval: 1>
      <h1>{audiences[audiencesNum].title}</h1> 
  </Typewriter>
   <button on:click={() => spendToken(3)}>Lock</button>
{:else}
    <h1>{audiences[audiencesNum].title}</h1> 
{/if}



<button on:click={spinTheWheel} class="is-primary">Spin the wheel</button>
