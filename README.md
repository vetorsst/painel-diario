# Painel Ritmo 40K · Vetor SST

Painel de ritmo comercial em arquivo único: mostra a meta do dia, os ritmos
necessários para alcançá-la, o histórico completo de lançamentos e comemora
quando a meta é batida.

## Como usar

Abra `index.html` direto no navegador — não precisa build, servidor
nem instalação. Para deixar no ar, basta subir o arquivo em qualquer hospedagem
estática (GitHub Pages, Netlify, Vercel, Hostinger ou a pasta `public/` do seu
servidor).

### Lançar, corrigir e apagar

- **Lançar** uma venda pelo painel "Lançar venda": valor, vendedor e cliente.
- **Corrigir** um lançamento pelo botão ✎, tanto na lista "Últimos lançamentos"
  quanto no histórico. Dá para mudar valor, hora, vendedor e cliente. Ao mudar a
  hora, o lançamento se reposiciona no bloco certo sem trocar de dia.
- **Apagar** pelo botão ×, que pede confirmação antes.

### Metas

O painel "Metas" tem dois campos:

- **Meta de hoje** — vale só para o dia de hoje.
- **Meta padrão dos outros dias** — vale para todo dia que não tiver a sua
  própria meta, inclusive os dias que ainda vêm.

Cada dia guarda a meta que valia nele, então trocar a meta hoje não reescreve o
julgamento dos dias antigos: no histórico, cada dia continua sendo comparado com
a meta que estava valendo naquele dia. O botão "Voltar hoje para a padrão"
remove a meta específica do dia.

### Ritmos necessários

O painel "Ritmos necessários" mostra, e recalcula a cada lançamento e a cada
troca de meta:

- **Ritmo ideal do dia** — a meta dividida pelos 10 blocos entre 9h e 19h.
- **Ritmo necessário agora** — quanto por hora é preciso vender no tempo que
  ainda resta para fechar o que falta.
- **A cada 30 e a cada 15 minutos** — o mesmo ritmo em pedaços menores.
- **Ritmo realizado** — o ritmo que a equipe está mantendo de fato.
- **Quanto cada bloco que ainda vem precisa trazer** — o que falta distribuído
  pelos blocos restantes, com o bloco corrente entrando só pelos minutos que
  ainda sobram nele.

## Configuração

A configuração fica no início da tag `<script>` do arquivo, em `var CONFIG`:

| Chave | O que faz |
| --- | --- |
| `META` | Meta padrão do dia, em reais (padrão: `40000`). O que for salvo no painel "Metas" manda nisto. |
| `API_URL` | Endpoint que guarda os lançamentos. Vazio: cada navegador guarda os seus no `localStorage`. Preenchido: todo mundo vê o mesmo número. |
| `API_TOKEN` | Opcional, enviado como `Authorization: Bearer ...`. |
| `SYNC_SEGUNDOS` | Intervalo de sincronização com a API (padrão: `15`). |

### Contrato da API

Dois métodos na mesma URL:

- `GET` devolve `{"meta":40000,"metas":{...},"lancamentos":[ ... ]}` (ou só o array).
- `POST` recebe `{"meta":40000,"metas":{...},"lancamentos":[ ... ]}` e grava tudo.

Onde:

- `meta` é a meta padrão, usada em todo dia sem meta própria.
- `metas` guarda as metas de dias específicos: `{"2026-08-31": 45000}`.
- cada lançamento tem o formato
  `{id, ts, dia:"AAAA-MM-DD", hora, min, valor, vendedor, cliente}` — basta
  devolver `id`, `ts` e `valor`, já que `dia`, `hora` e `min` são deduzidos do `ts`.

> Não versione tokens: deixe `API_TOKEN` vazio no arquivo publicado.

## Publicação

O site está hospedado no Netlify, conectado a este repositório: cada `git push`
na branch `main` republica o painel automaticamente. O `netlify.toml` já diz que
não há nada para compilar — o Netlify apenas serve a raiz do repositório, e o
`index.html` é o painel.

O repositório é privado; só o site fica público. Vale lembrar que, com
`API_URL` vazio, cada navegador guarda os seus próprios lançamentos no
`localStorage` — para a equipe inteira ver o mesmo número é preciso configurar
o `API_URL`.
