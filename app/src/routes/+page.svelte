<script lang="ts">
  import { generateSecretKey, getPublicKey, nip19 } from 'nostr-tools'
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
</div>
