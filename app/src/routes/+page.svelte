<script lang="ts">
  let name = 'João'
  let age = 26

  $: isAdult = age >= 18

  function increaseAge() {
    age += 1
  }

  function decreaseAge() {
    if (age === 0) return alert('Idade não pode ser menor que 0')

    age -= 1
  }
</script>

<div>
  <h1>Svelte.dev basics</h1>

  <a href="https://svelte.dev/" target="_blank">Docs</a>

  <hr>

  <h2>Variáveis</h2>

  <p>Meu nome é { name } e eu tenho { age } anos.</p>

  <h2>Funções e event listeners</h2>

  <div>
    <button on:click={increaseAge}>aumentar idade</button>
  </div>

  <div>
    <button on:click={decreaseAge}>diminuir idade</button>
  </div>

  <h2>Condicionais e computed properties</h2>

  <ul>
    <li>É maior de idade: { isAdult ? 'Sim' : 'Não' }</li>
  </ul>

  {#if isAdult}
    <p>É maior de idade</p>
  {:else}
    <p>Não é maior de idade</p>
  {/if}

  <h2>Input de texto</h2>
  Nome:
  <input bind:value={name} />
</div>
