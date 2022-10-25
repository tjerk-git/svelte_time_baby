<script>
  import Typewriter from "svelte-typewriter";
  import { fade } from "svelte/transition";
  import ShareSheet from "./ShareSheet.svelte";
  import Modal from "./Modal.svelte";

  import tasks from "../tasks.json";
  import contexts from "../contexts.json";
  import audiences from "../audiences.json";

  let taskNumLocked = false;
  let contextsNumLocked = false;
  let audiencesNumLocked = false;
  let showModal = false;

  let tasksNum = 1;
  let contextsNum = 1;
  let audiencesNum = 1;
  let tokens = 2;
  let challenges = [];

  const share = () => {
    showModal = true;
    challenges = [
      { title: tasks[tasksNum].title },
      { title: contexts[contextsNum].title },
      { title: audiences[audiencesNum].title },
    ];
  };

  const spinTheWheel = () => {
    if (taskNumLocked === false) {
      tasksNum = Math.floor(Math.random() * tasks.length);
    }
    if (contextsNumLocked === false) {
      contextsNum = Math.floor(Math.random() * contexts.length);
    }

    if (audiencesNumLocked === false) {
      audiencesNum = Math.floor(Math.random() * audiences.length);
    }
  };

  /**
   * @param {number} id
   */
  function spendToken(id) {
    if (tokens > 0) {
      tokens = tokens - 1;
    } else {
      return;
    }

    if (id === 1) {
      taskNumLocked = true;
    }

    if (id === 2) {
      contextsNumLocked = true;
    }

    if (id === 3) {
      audiencesNumLocked = true;
    }
  }
</script>

<h1>
  Tokens:
  {#each [tokens] as c (c)}
    <strong in:fade>{c}</strong>
  {/each}
</h1>

{#if taskNumLocked == false}
  <Typewriter mode="scramble" wordInterval: 1>
    <h1>{tasks[tasksNum].title}</h1>
  </Typewriter>
  // component for button with hide if no tokens
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
<button on:click={share} class="is-primary">Save results</button>

{#if showModal}
  <Modal on:close={() => (showModal = false)}>
    <h2 slot="header">
      Share
      <small><em>adjective</em> mod·al \ˈmō-dəl\</small>
    </h2>
    <ShareSheet {challenges} />
  </Modal>
{/if}
