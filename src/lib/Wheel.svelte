<script lang="ts">
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

<br />

<div class="card">
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
</div>

<br /><br />
<button on:click={spinTheWheel} class="is-primary button">Spin the wheel</button
>
<br />
<button on:click={share} class="is-primary green">Save results</button>

{#if showModal}
  <Modal on:close={() => (showModal = false)}>
    <h2 slot="header">
      <small><em>Sharing is caring</em></small>
    </h2>
    <ShareSheet {challenges} />
  </Modal>
{/if}

<style>
  .card {
    border: 2px solid salmon;
    padding: 1rem;
    border-radius: 15px;
  }

  .green {
    background-color: rgb(82, 129, 209);
    color: white;
    font-size: 1.2rem;
  }

  .button {
    background-color: #e73dc8;
    border: 0 solid #e5e7eb;
    box-sizing: border-box;
    color: #000000;
    display: flex;
    font-family: ui-sans-serif, system-ui, -apple-system, system-ui, "Segoe UI",
      Roboto, "Helvetica Neue", Arial, "Noto Sans", sans-serif,
      "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol",
      "Noto Color Emoji";
    font-size: 1.6rem;
    font-weight: 700;
    justify-content: center;
    line-height: 1.75rem;
    padding: 0.75rem 1.65rem;
    position: relative;
    text-align: center;
    text-decoration: none #000000 solid;
    text-decoration-thickness: auto;
    width: 100%;
    max-width: 460px;
    position: relative;
    cursor: pointer;
    transform: rotate(-2deg);
    user-select: none;
    -webkit-user-select: none;
    touch-action: manipulation;
  }

  .button:focus {
    outline: 0;
  }

  .button:after {
    content: "";
    position: absolute;
    border: 1px solid #000000;
    bottom: 4px;
    left: 4px;
    width: calc(100% - 1px);
    height: calc(100% - 1px);
  }
</style>
