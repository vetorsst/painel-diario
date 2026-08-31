# Painel Ritmo 40K · Vetor SST

Painel de ritmo comercial em arquivo único: mostra a meta do dia, o ritmo
necessário por hora (9h–19h), o histórico completo de lançamentos e comemora
quando a meta é batida.

## Como usar

Abra `painel-ritmo-40k.html` direto no navegador — não precisa build, servidor
nem instalação. Para deixar no ar, basta subir o arquivo em qualquer hospedagem
estática (GitHub Pages, Netlify, Vercel, Hostinger ou a pasta `public/` do seu
servidor).

## Configuração

Toda a configuração fica no início da tag `<script>` do arquivo, em `var CONFIG`:

| Chave | O que faz |
| --- | --- |
| `META` | Meta do dia, em reais (padrão: `40000`). |
| `API_URL` | Endpoint que guarda os lançamentos. Vazio: cada navegador guarda os seus no `localStorage`. Preenchido: todo mundo vê o mesmo número. |
| `API_TOKEN` | Opcional, enviado como `Authorization: Bearer ...`. |
| `SYNC_SEGUNDOS` | Intervalo de sincronização com a API (padrão: `15`). |

### Contrato da API

Dois métodos na mesma URL:

- `GET` devolve `{"meta":40000,"lancamentos":[ ... ]}` (ou só o array).
- `POST` recebe `{"meta":40000,"lancamentos":[ ... ]}` e grava tudo.

Cada lançamento tem o formato
`{id, ts, dia:"AAAA-MM-DD", hora, min, valor, vendedor, cliente}` — basta
devolver `id`, `ts` e `valor`, já que `dia`, `hora` e `min` são deduzidos do `ts`.

> Não versione tokens: deixe `API_TOKEN` vazio no arquivo publicado.
