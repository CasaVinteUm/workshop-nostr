<script lang="ts">
  import type { EventTemplate, Event } from 'nostr-tools'
  import { generateSecretKey, getPublicKey, nip19, getEventHash, finalizeEvent } from 'nostr-tools'
  import { bytesToHex } from '@noble/hashes/utils'

  let sk: Uint8Array | null = null
  let pk: string | null = null

  let isImportingSk = false
  let inputSk = ''

  $: skHex = sk ? bytesToHex(sk) : null
  $: skNip19 = sk ? nip19.nsecEncode(sk) : null
  $: pkNip19 = pk ? nip19.npubEncode(pk) : null

  function generateKeys() {
    isImportingSk = false
    sk = generateSecretKey()
    pk = getPublicKey(sk)
  }

  function showImportSk() {
    sk = null
    pk = null
    isImportingSk = true
  }

  function importSk() {
    try {
      const { data, type } = nip19.decode(inputSk)

      if (type !== 'nsec') {
        throw new Error('Invalid key type')
      }

      sk = data
      pk = getPublicKey(sk)
      inputSk = ''
      isImportingSk = false
    } catch (error) {
      alert('Chave privada inválida')
    }
  }

  let event: Event

  let inputContent = ''

  function updateContent() {
    if (!sk) {
      alert('Chave privada não encontrada')
      return
    }

    const eventTemplate: EventTemplate = {
      content: inputContent,
      created_at: Math.floor(Date.now() / 1000),
      kind: 1,
      tags: []
    }

    event = finalizeEvent(eventTemplate, sk)
  }
</script>

<div>
  <h1>Chave privada (sk) e Chave pública (pk)</h1>

  <button on:click={generateKeys}>Gerar chaves</button>
  <button on:click={showImportSk}>Importar chave privada</button>

  {#if sk !== null && pk !== null }
    <pre>
      # Chave privada (sk) hex:
      {skHex}
      # Chave pública (pk) hex:
      {pk}

      # Chave privada (sk) nip19:
      {skNip19}
      # Chave pública (pk) nip19:
      {pkNip19}
    </pre>
  {/if}

  {#if isImportingSk}
    <p>Chave privada (sk) hex:</p>
    <input type="text" bind:value={inputSk} />
    <button on:click={importSk}>
      Importar
    </button>
  {/if}

  <h1>Eventos e assinaturas</h1>

  <p>Conteúdo:</p>
  <input bind:value={inputContent} />
  <button on:click={updateContent}>atualizar e assinar</button>

  {#if event}
    <pre>{JSON.stringify(event, null, 2)}</pre>
  {/if}
</div>

<style>
pre {
  font-family: monospace;
  white-space: pre-wrap;
  display: block;
}
</style>
