# Painel Comercial · Vetor SST

Painel de ritmo comercial em arquivo único: mostra a meta do mês, quanto falta
para bater, o ritmo atual e o ritmo diário necessário, com histórico completo de
lançamentos e comemoração ao bater a meta.

**No ar em https://vetorsst.github.io/painel-diario/**

## Como usar

Abra `index.html` direto no navegador — não precisa build, servidor nem
instalação. Para deixar no ar, basta subir o arquivo em qualquer hospedagem
estática.

### O que o painel mostra

O topo traz o realizado do mês contra a meta, com barra de progresso. Abaixo,
quatro números:

| | |
| --- | --- |
| **Falta para bater** | quanto ainda falta, e quantos dias úteis restam |
| **Ritmo atual** | o realizado dividido pelos dias úteis já decorridos |
| **Ritmo necessário** | o que falta dividido pelos dias úteis que restam |
| **Hoje** | o que já entrou hoje, contra o que hoje precisa trazer |

Não existe meta diária fixa: **a meta do dia é o próprio ritmo necessário**, que
se reajusta a cada venda lançada e a cada dia que passa. Dia útil é de segunda a
sexta; feriados não são descontados.

### Lançar, corrigir e apagar

- **Lançar** uma venda pelo quadro "Lançar venda": valor, vendedor e cliente.
- **Corrigir** pelo botão ✎, tanto na lista do painel quanto no histórico. Dá
  para mudar valor, hora, vendedor e cliente. Ao mudar a hora, o lançamento se
  reposiciona sem trocar de dia.
- **Apagar** pelo botão ×, que pede confirmação antes.

### Meta do mês

O quadro "Meta do mês" tem um campo só. O que for salvo ali passa a valer no
lugar do `META_MES` da configuração, e todo o resto do painel — ritmo
necessário, quanto falta, meta de hoje — sai dele.

## Configuração

A configuração fica no início da tag `<script>` do arquivo, em `var CONFIG`:

| Chave | O que faz |
| --- | --- |
| `META_MES` | Meta do mês, em reais (padrão: `250000`). O que for salvo no painel manda nisto. |
| `API_URL` | Endpoint que guarda o estado. Vazio: cada navegador guarda o seu no `localStorage`. Preenchido: todo mundo vê o mesmo número. |
| `API_TOKEN` | Opcional, enviado como `Authorization: Bearer ...`. |
| `SYNC_SEGUNDOS` | Intervalo de sincronização com a API (padrão: `15`). |

### Contrato da API

Dois métodos na mesma URL:

- `GET` devolve `{"metaMes":250000,"lancamentos":[ ... ]}` (ou só o array).
- `POST` recebe `{"metaMes":250000,"lancamentos":[ ... ]}` e grava tudo.

Cada lançamento tem o formato
`{id, ts, dia:"AAAA-MM-DD", hora, min, valor, vendedor, cliente}` — basta
devolver `id`, `ts` e `valor`, já que `dia`, `hora` e `min` são deduzidos do `ts`.

> Não versione tokens: deixe `API_TOKEN` vazio no arquivo publicado.

## Publicação

O painel está no **GitHub Pages**, servindo a branch `main` a partir da raiz —
o mesmo caminho do `relatorio-semanal-vetor`. Cada `git push` na `main`
republica em cerca de um minuto. O `.nojekyll` desliga o processamento Jekyll,
que não tem serventia num site de arquivo único.

## Duas versões

Existe também uma versão deste painel como Artifact do Claude, que é a cópia
usada no dia a dia. As duas têm **persistências diferentes e não são
intercambiáveis**: o Artifact salva republicando a si mesmo, enquanto esta
versão usa `localStorage` e a camada opcional de `API_URL`. Copiar o arquivo de
um lado para o outro quebra o que foi copiado — mudanças precisam ser portadas.
