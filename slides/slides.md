---
theme: default
background: https://i.imgur.com/jHU4SY1.png
title: NOSTR
highlighter: shiki
drawings:
  persist: false
mdc: true
layout: cover
transition: fade
---

# NOSTR

Faça seu primeiro app NOSTR.

<div class="uppercase text-sm tracking-widest">
jaonoctus & vmstabile
</div>

<div class="abs-bl mx-14 my-12 flex">
  <img src="https://i.imgur.com/pISUX4M.png" class="h-8">
  <div class="ml-3 flex flex-col text-left">
    <div><b>Casa21</b></div>
    <div class="text-sm opacity-50">Workshop powered by Vinteum - May. 3th, 2024</div>
  </div>
</div>

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/jaonoctus" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

---
transition: fade
layout: fact
---

# NOSTR

Notes and Other Stuff Transmitted by Relays

---
transition: fade
layout: fact
---

# NIPs

Nostr Implementation Possibilities


---
transition: fade
---

# NIP-01

Basic protocol flow description

### Eventos e assinaturas

````md magic-move
```js
{}
```
```js
{
  "content": "texto arbitrário",
}
```
```js
{
  "content": "texto arbitrário",
  "kind": "número inteiro entre 0 e 65535",
}
```
```js
{
  "content": "texto arbitrário",
  "kind": "número inteiro entre 0 e 65535",
  "created_at": "unix timestamp em segúndos",
}
```
```js
{
  "content": "texto arbitrário",
  "kind": "número inteiro entre 0 e 65535",
  "created_at": "unix timestamp em segúndos",
  "tags": [],
}
```
```js
{
  "content": "texto arbitrário",
  "kind": "número inteiro entre 0 e 65535",
  "created_at": "unix timestamp em segúndos",
  "tags": [
    ["chave", "valor"],
  ],
}
```
```js
{
  "content": "texto arbitrário",
  "kind": "número inteiro entre 0 e 65535",
  "created_at": "unix timestamp em segúndos",
  "tags": [
    ["chave", "valor"]
  ],
  "pubkey": "chave pública do autor do evento",
}
```
```js
{
  "content": "texto arbitrário",
  "kind": "número inteiro entre 0 e 65535",
  "created_at": "unix timestamp em segúndos",
  "tags": [
    ["chave", "valor"]
  ],
  "pubkey": "chave pública do autor do evento",
  "id": "hash sha256 do conteúdo serializado acima",
}
```
```js
{
  "content": "texto arbitrário",
  "kind": "número inteiro entre 0 e 65535",
  "created_at": "unix timestamp em segúndos",
  "tags": [
    ["chave", "valor"]
  ],
  "pubkey": "chave pública do autor do evento",
  "id": "hash sha256 do conteúdo serializado acima",
  "sig": "assinatura do id com a chave privada do autor do evento"
}
```
````


---
transition: fade
---

# NIP-01

Basic protocol flow description

### Comunicação entre clientes e relays (Websocket)

Os clientes podem enviar 3 tipos de mensagens, que devem ser JSON arrays, de acordo com os seguintes padrões:

- `["EVENT", "evento"]` <small>, usado para publicar eventos.</small>
- `["REQ", "id", "filtro1", "filtro2", ...]` <small>, usado para solicitar eventos e escutar novas atualizações.</small>
- `["CLOSE", "id"]` <small>, usado para parar de ouvir atualizações.</small>

---
transition: fade
---

# Tipos de evento

|  kind   |  descrição   |   NIP   |
| --- | --- | --- |
| <kbd>0</kbd>  | Metadata |  [01](https://github.com/nostr-protocol/nips/blob/master/01.md)  |
| <kbd>1</kbd>  | Short Text Note |  [01](https://github.com/nostr-protocol/nips/blob/master/01.md)  |
| <kbd>3</kbd>  | Follows |  [02](https://github.com/nostr-protocol/nips/blob/master/02.md)  |
| <kbd>6</kbd>  | Repost |  [18](https://github.com/nostr-protocol/nips/blob/master/18.md)  |
| <kbd>7</kbd>  | Reaction |  [25](https://github.com/nostr-protocol/nips/blob/master/25.md)  |
| <kbd>9735</kbd>  | Zap |  [57](https://github.com/nostr-protocol/nips/blob/master/57.md)  |
| [Entre outros](https://github.com/nostr-protocol/nips?tab=readme-ov-file#event-kinds) |

---
transition: fade
---

# Mão na massa!

https://github.com/CasaVinteUm/workshop-nostr

````md magic-move
```bash
# clone o projeto
git clone git@github.com:CasaVinteUm/workshop-nostr.git
```
```bash
# clone o projeto
git clone git@github.com:CasaVinteUm/workshop-nostr.git

# faça o checkout para o checkpoint
git checkout checkpoint-01
```
```bash
# clone o projeto
git clone git@github.com:CasaVinteUm/workshop-nostr.git

# faça o checkout para o checkpoint
git checkout checkpoint-01

# entre no projeto Svelte
cd app
```
```bash
# clone o projeto
git clone git@github.com:CasaVinteUm/workshop-nostr.git

# faça o checkout para o checkpoint
git checkout checkpoint-01

# entre no projeto Svelte
cd app

# instale as dependências
npm i
```
```bash
# clone o projeto
git clone git@github.com:CasaVinteUm/workshop-nostr.git

# faça o checkout para o checkpoint
git checkout checkpoint-01

# entre no projeto Svelte
cd app

# instale as dependências
npm i

# rode o web app localmente
npm run dev
```
````

---
transition: fade
layout: center
---

# Obrigado
