# Playlist Creator

Aplicativo web para criar e organizar playlists grandes no Spotify. Roda inteiro no navegador (um único `index.html`), sem servidor próprio.

**Online:** https://vicckitiki.github.io/Playlist_Creator/

## Funcionalidades

- **Criar nova playlist** a partir de um gênero ou artista.
- **Adicionar a uma playlist existente**, ignorando músicas que ela já tem (inclusive versões diferentes da mesma música).
- **Mesclar playlists**: compara duas playlists e adiciona só o que falta, salvando na playlist 1, na playlist 2 ou em uma nova.
- **Preview antes de salvar**, para conferir o resultado.

## Tipos de busca

| Tipo | O que faz |
| --- | --- |
| Por gênero | Encontra artistas classificados no gênero pelo Spotify e confere o gênero de cada um. Use o nome em inglês, como o Spotify (ex.: `gregorian chant`, `indie rock`, `bossa nova`). |
| Parecidos com um artista | Usa os gêneros mais específicos do artista de referência para achar outros artistas parecidos e inclui o próprio artista. |
| Só um artista | Traz apenas as músicas do próprio artista. |

Em todos os tipos, só entram faixas em que o artista é realmente o dono da música, o que elimina covers feitos por terceiros. Também é possível:

- **Evitar covers, tributos e karaokê** (ligado por padrão).
- **Evitar versões ao vivo, remix, demo e instrumental.**
- Definir a **quantidade de músicas** e o **máximo por artista**, para variar o resultado.

## Como a mesclagem compara as músicas

Duas faixas são consideradas a mesma música quando o título-base é igual e elas têm ao menos um artista em comum. Para isso, o título é normalizado e ignora:

- versões ao vivo, acústicas, remix, remaster, demo e instrumental;
- participações (`feat.`, `ft.`, `with`);
- anos, acentos e pontuação.

Parênteses que fazem parte do nome da música (como em "(Don't Fear) The Reaper") são mantidos. Faixas marcadas como cover, tribute ou karaoke são comparadas só pelo título. Há ainda uma opção para tratar o mesmo título com artistas diferentes como a mesma música (pega mais covers, mas pode descartar músicas diferentes com nome igual).

## Requisitos

- Conta Spotify **Premium** (exigência do Spotify para apps em Development Mode).
- Um app criado no [Spotify Developer Dashboard](https://developer.spotify.com/dashboard).
- Seu e-mail cadastrado em **Settings → User Management** do app (Development Mode permite poucos usuários).

## Configuração

No topo do script do `index.html`:

```js
const CLIENT_ID = 'seu_client_id';
const REDIRECT_URI = 'https://seu-usuario.github.io/seu-repositorio/';
```

O `REDIRECT_URI` precisa ser **idêntico** ao cadastrado em *Redirect URIs* no Dashboard. O app usa OAuth 2.0 com PKCE e os escopos `playlist-modify-public`, `playlist-modify-private` e `playlist-read-private`.

## Publicação

O projeto é estático. Para publicar no GitHub Pages, ative *Settings → Pages* apontando para a branch principal.

## Privacidade

Não há backend. O token de acesso fica apenas no `localStorage` do seu navegador e é apagado ao clicar em **Sair**. As playlists criadas são privadas por padrão.

## Limitações

- Só aparecem e podem ser alteradas playlists **que são suas**.
- O Spotify limita a busca a 10 resultados por requisição e impõe cota de requisições a apps em Development Mode. Buscas muito grandes podem retornar menos músicas do que o pedido.
- A classificação de gêneros é do Spotify e pode conter erros; um cover de uma banda mal classificada pode passar pelos filtros.
- Prévias de áudio não são exibidas, pois o Spotify deixou de fornecê-las.
