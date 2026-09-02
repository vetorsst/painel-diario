# Painel Comercial · Vetor SST

Painel de ritmo comercial em arquivo único: mostra a meta do mês, quanto falta
para bater, o ritmo atual e o ritmo diário necessário, com histórico completo de
lançamentos e comemoração ao bater a meta. As receitas podem vir sozinhas da
[planilha de conciliação bancária](#conciliação-bancária), com o nome de quem
pagou.

**No ar em https://vetorsst.github.io/painel-diario/**

## Dois endereços, duas funções

| | |
| --- | --- |
| `…/painel-diario/?tv` | **A televisão.** Só leitura. Fica aberta o dia inteiro no PC ligado na TV. |
| `…/painel-diario/` | **O computador.** Mesmo painel, com o que dá para mexer. |

O `?tv` é tela de parede: some o formulário de lançar venda, somem os botões ✎ e
×, some o campo de meta e o botão de histórico. Sobra o número grande, a barra
do dia, três indicadores do mês e a lista de quem pagou. Tipografia dimensionada
para leitura a 3–6 metros, e o layout inteiro cabe numa tela sem rolagem.

Sem o `?tv`, o mesmo arquivo abre o painel completo. Com a planilha ligada, ele
deixa de ser o lugar onde se digita venda e passa a ser a régua:

- **Meta do mês** — o único campo que ainda precisa de gente. O ritmo necessário
  e a meta do dia saem dele.
- **Corrigir** (✎) uma linha que veio errada da planilha: o pagador não é o
  cliente, o valor veio bruto, o nome está impossível. A linha corrigida descola
  da planilha e para de ser sobrescrita.
- **Apagar** (×) uma linha que não deveria contar como venda. Ela não volta na
  próxima leitura.
- **Histórico** — todos os dias, com busca por cliente ou valor e exportação em
  CSV. É onde se confere o mês fechado.
- **Lançar venda** — continua existindo, mas com a conciliação ligada quase
  nunca é o certo: o painel mede dinheiro que caiu na conta, e o que for
  digitado à mão vai contar em dobro quando a mesma receita chegar pela
  planilha. Use só para o que nunca vai passar pelo banco.

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

## Modo TV

Acrescente `?tv` ao endereço — `https://vetorsst.github.io/painel-diario/?tv` — e
o painel vira tela de parede: sem formulário de lançar venda, sem ✎ e ×, sem
campo de meta, sem histórico. Só o que se lê de longe. Sem o `?tv`, o painel
continua igual ao de sempre, para corrigir alguma coisa pelo computador.

### O que a TV mostra

O número grande é a **meta do dia**, porque é o que a equipe consegue mudar
hoje. Abaixo dele, a barra do dia; depois, o mês contra a meta, quanto falta e
quantos recebimentos entraram hoje; depois, quem pagou.

A meta do dia é o que falta dividido pelos dias úteis restantes, **congelada no
realizado de ontem**. Congelar importa: o ritmo necessário se recalcula a cada
venda e por isso encolhe durante o expediente — bom como bússola do mês, péssimo
como alvo do dia, porque o alvo fugiria junto com o dinheiro que entra. Congelado,
o número não muda debaixo de quem está olhando e a barra enche de verdade.

A tarja de situação compara o realizado com o **esperado até esta hora** (das 8h
às 18h), não com o dia inteiro: às 9h ninguém está atrasado por ter R$ 0 na
conta. E quando ainda não entrou nada com a planilha ligada, ela diz
**aguardando planilha** em vez de acusar atraso — o painel não sabe distinguir
dia fraco de conciliação que ainda não chegou.

Quando não entrou nada hoje, o número grande vira a própria meta e a legenda
aponta a última entrada real ("última entrada seg 31/08, R$ 17.498"), em vez de
um `R$ 0` gigante que faz a tela parecer quebrada. Em sábado, domingo e feriado
não há meta do dia: o mês assume o número grande.

### O que a TV faz sozinha

| | |
| --- | --- |
| Relógio | anda de minuto em minuto — na TV os segundos piscam 86.400 vezes por dia sem informar nada |
| Festa da meta batida | toca por 90 s e sai de cena; sem isso os fogos rodariam a 60 fps até o fim do mês, porque ninguém clica |
| Planilha sem resposta | tarja vermelha no topo depois de 20 min sem leitura, mais o motivo do erro |
| Aba volta a aparecer | relê a planilha na hora, em vez de confiar no que o navegador congelou |
| 03:30 da manhã | recarrega a página: é o que faz versão nova chegar à tela e o que tira o painel de um travamento sem ninguém por perto |

## Conciliação bancária

Com `CSV_RECEITAS` preenchido, o painel lê a planilha de conciliação sozinho e
ninguém precisa lançar receita a receita. Cada linha com `Categoria = Receita`
vira um lançamento, com o **nome de quem pagou** no lugar do cliente, e essas
linhas aparecem com o selo **planilha**.

Como a conciliação guarda o dia e não a hora, o lançamento que vem dela mostra
um traço no lugar do relógio, em vez de fingir uma hora que ninguém marcou.

### Publicando só as receitas

A conciliação inteira tem salário, despesa e transferência — nada disso pode ir
para um link público. Por isso o que se publica é **uma aba derivada**, só com
as receitas, e não o documento inteiro.

1. Na planilha mestre, crie uma aba chamada `painel`.
2. Em `painel!A1`, cole a fórmula (trocando `dados` pelo nome da aba onde a
   conciliação mora):

   ```
   =QUERY(dados!A:I;"select B,C,D,E,G,H where H='Receita'";1)
   ```

   Em planilha com locale inglês, troque o `;` por `,`.
3. **Arquivo → Compartilhar → Publicar na web.**
4. Em "Conteúdo publicado", escolha **a aba `painel`** — nunca "Documento
   inteiro" — e o formato **Valores separados por vírgula (.csv)**.
5. Deixe marcado "Republicar automaticamente quando forem feitas alterações",
   publique e copie o link.
6. Cole o link em `CSV_RECEITAS`, no início da tag `<script>` do `index.html`.

Publicar na web é independente do compartilhamento da planilha: quem tiver o
link do CSV vê a aba `painel` e mais nada. O Google guarda o CSV publicado em
cache por alguns minutos, que é o motivo de `CSV_SEGUNDOS` ser 300.

### Colunas que o painel procura

Ele acha as colunas pelo nome do cabeçalho, em qualquer ordem, com ou sem
acento: `DATA` e `VALOR` são obrigatórias; `NOME`, `BANCO`, `DESCRIÇÃO` e
`Categoria` entram se existirem. Data em `DD/MM/AAAA` ou `AAAA-MM-DD`; valor em
`1.060,00` ou `1060.00`. Valor negativo ou zerado é ignorado, então taxa de pix,
taxa de cartão e despesa ficam de fora sozinhas.

Se a aba publicada já vem filtrada e **sem** a coluna `Categoria`, o painel
aceita toda entrada positiva que sobrou — que é justamente o que a aba se
propôs a listar. Se o CSV não tiver `DATA` e `VALOR`, ele avisa no rodapé e não
mexe em nada, em vez de zerar o mês.

### Reler a planilha não bagunça nada

O id de cada lançamento sai do conteúdo da própria linha — dia, banco, nome,
descrição e valor — então:

| Aconteceu | O painel faz |
| --- | --- |
| Reler a mesma planilha | nada muda, nem duplica |
| Linha nova na planilha | entra sozinha |
| Valor corrigido na planilha | se propaga para o painel |
| Linha sai da planilha | some do painel |
| Você apaga a linha no painel | ela não volta na próxima leitura |
| Você corrige a linha pelo ✎ | ela descola da planilha e a sua versão manda |
| Lançamento digitado à mão | nunca é tocado |

O rodapé mostra quantas receitas foram lidas e a que horas — ou o motivo, se a
leitura falhar.

Há uma trava contra leitura estragada: se um lote novo fizer sumir mais de 30%
das linhas que vieram da planilha, o painel **recusa a leitura** e acende o
alerta em vez de aplicar. Um CSV cortado no meio do download, ou a `=QUERY`
virando `#REF!`, devolve um resultado válido e curto — e sem essa trava o mês
inteiro evaporaria da tela sem ninguém perceber.

### Contando duas vezes

Quando a planilha passa a cobrir um dia em que já havia lançamento digitado à
mão, os dois somam e o mês incha. O painel não escolhe sozinho o que apagar:
mostra um aviso acima do número grande, dizendo quantos lançamentos manuais
caem em dias que a planilha já cobre e quanto isso representa, com um botão para
apagar só esses.

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
| `CSV_RECEITAS` | URL do CSV publicado da conciliação bancária. Vazio: nada muda e tudo é lançado à mão. Preenchido: as receitas entram sozinhas. |
| `CSV_SEGUNDOS` | Intervalo de leitura da planilha (padrão: `300`). |

### Contrato da API

Dois métodos na mesma URL:

- `GET` devolve `{"metaMes":250000,"lancamentos":[ ... ]}` (ou só o array).
- `POST` recebe `{"metaMes":250000,"lancamentos":[ ... ]}` e grava tudo.

Cada lançamento tem o formato
`{id, ts, dia:"AAAA-MM-DD", hora, min, valor, vendedor, cliente, origem}` —
basta devolver `id`, `ts` e `valor`, já que `dia`, `hora` e `min` são deduzidos
do `ts`. `origem` vale `"csv"` na linha que veio da conciliação e vazio na
digitada à mão.

O corpo também carrega `ocultos`: os ids de linhas da planilha que foram
apagadas no painel e não devem voltar na próxima leitura.

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
