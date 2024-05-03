<script lang="ts">
  import type { EventTemplate, Event, Subscription } from 'nostr-tools'
  import { generateSecretKey, getPublicKey, nip19, finalizeEvent, validateEvent } from 'nostr-tools'
  import { bytesToHex } from '@noble/hashes/utils'
  import { Relay } from 'nostr-tools/relay'

  // 01 Chave privada (sk) e Chave pública (pk)

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

  // 02 Eventos e assinaturas

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

  // 03 Relays

  const relayURL = 'wss://relay.damus.io/'
  let relay: Relay | null = null
  $: isConnected = relay !== null && relay.connected

  async function connectToRelay() {
    try {
      relay = await Relay.connect(relayURL)
    } catch (error) {
      alert('Erro ao conectar ao relay')
    }
  }

  async function publishEvent() {
    if (!isConnected) {
      alert('Relay não conectado')
      return
    }

    try {
      await relay?.publish(event)
      alert('Evento publicado com sucesso')
    } catch (error) {
      alert('Erro ao publicar evento')
    }
  }

  let inputSubscribePk = ''
  let subscribedPks: string[] = []
  let subs: Subscription
  let events: Event[] = []

  function subscribeToPk() {
    if (!relay) {
      return
    }

    try {
      if (subscribedPks.includes(inputSubscribePk)) {
        return
      }
      const { type, data } = nip19.decode(inputSubscribePk)

      if (type !== 'npub') {
        throw new Error('Invalid key type')
      }

      subscribedPks = [...subscribedPks, inputSubscribePk]

      subs = relay?.subscribe([
        {
          kinds: [1],
          authors: [
            data
          ]
        }
      ], {
        onevent(event) {
          events = [...events, event]
        }
      })
    } catch (error) {
      alert('Chave pública inválida')
      return
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

  <h1>Eventos e assinaturas</h1>

  <p>Conteúdo:</p>
  <input bind:value={inputContent} />
  <button on:click={updateContent}>atualizar e assinar</button>

  {#if event}
    <pre>{JSON.stringify(event, null, 2)}</pre>
  {/if}

  <h1>Relays</h1>

  <pre>
    Conectado: {isConnected ? 'Sim' : 'Não'}
    Ouvindo:
    {#each subscribedPks as pk}
      <p> - {pk}</p>
    {/each}
  </pre>

  {#if !isConnected}
    <button on:click={connectToRelay}>Conectar</button>
  {/if}

  {#if isConnected}
    {#if validateEvent(event)}
      <button on:click={publishEvent}>Publicar nota</button>
    {/if}

    <p>Chave pública para ouvir:</p>
    <input type="text" bind:value={inputSubscribePk} />
    <button on:click={subscribeToPk}>ouvir este perfil</button>

    <div>
      <ul>
        {#each events as event}
          <li>{new Date(event.created_at * 1000).toLocaleString()} <b>{nip19.npubEncode(event.pubkey).slice(0, 6)}...{nip19.npubEncode(event.pubkey).slice(-6)}</b> disse <b>{event.content}</b></li>
        {/each}
      </ul>
    </div>
  {/if}
</div>
