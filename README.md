# Painel Comercial · Vetor SST

Painel de ritmo comercial em arquivo único: mostra a meta do mês, quanto falta
para bater, o ritmo atual, o ritmo diário necessário e onde o mês fecha nesse
ritmo, com histórico completo de
lançamentos e comemoração ao bater a meta. As receitas podem vir sozinhas da
[planilha de conciliação bancária](#conciliação-bancária), com o nome de quem
pagou.

**No ar em https://vetorsst.github.io/painel-diario/**

## Dois endereços, duas funções

| | |
| --- | --- |
| `…/painel-diario/?tv` | **A televisão.** Só leitura. Fica aberta o dia inteiro no PC ligado na TV. |
| `…/painel-diario/` | **O computador.** Mesmo painel, com o que dá para mexer. |

O `?tv` é tela de parede: some o campo de meta, some o custo e some o botão de
histórico. Sobra o número grande, a barra do dia, três indicadores do mês e a
lista de quem pagou. Tipografia dimensionada para leitura a 3–6 metros, e o
layout inteiro cabe numa tela sem rolagem.

Sem o `?tv`, o mesmo arquivo abre o painel completo. Ele não é onde se lança
venda — é a régua:

- **Atualizar** — relê a planilha na hora, sem recarregar a página. O F5 jogaria
  fora o estado da tela (a meta sendo digitada, o filtro, o dia expandido no
  histórico) só para buscar um CSV de poucos kB. Sozinho, o painel
  relê a cada `CSV_SEGUNDOS`. O Google guarda o CSV publicado em cache por
  alguns minutos, então uma alteração recém-salva pode demorar a aparecer mesmo
  clicando — o botão pede uma URL diferente a cada vez para tentar furar isso,
  mas nem sempre funciona.
- **Meta e custo do mês** — os únicos campos que ainda precisam de gente. Da
  meta saem o ritmo necessário e a meta do dia; do custo, o lucro projetado.
- **Histórico** — todos os dias, com busca por cliente ou valor e exportação em
  CSV. É onde se confere o mês fechado.

Receita, não. **Não se lança, corrige nem apaga recebimento pelo painel** — nem
no computador, nem na TV. O que muda o número é a planilha financeira, e só
ela. Uma linha errada se conserta lá, e na próxima leitura o painel acompanha.

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
sexta, tirando os [feriados](#feriados).

### Projeção do mês

Abaixo dos quatro números, o quadro **Projeção do mês** responde onde o mês
termina se o ritmo atual se mantiver até o último dia útil:

| | |
| --- | --- |
| **Faturamento projetado** | o realizado mais o ritmo atual vezes os dias úteis que faltam depois de hoje; a nota diz quanto isso representa da meta |
| **Custo do mês** | o custo total do mês, informado no quadro "Meta do mês" |
| **Resultado projetado** | o lucro: faturamento projetado menos o custo do mês, com a margem sobre o faturamento |

A conta vai aberta embaixo do quadro, para quem olha poder refazê-la — por
exemplo, com um custo hipotético de R$ 150.000: "R$ 65.225 realizado +
R$ 8.153/dia × 13 dias úteis depois de hoje = R$ 171.216 de faturamento −
R$ 150.000 de custo = R$ 21.216 de resultado".

Sem custo informado, o quadro mostra o faturamento projetado e pede o custo. O
custo é um número só, sem nenhuma despesa linha a linha — mas, como o
repositório e o site são públicos, quem tiver o link vê o custo e o lucro.

Hoje não é contado duas vezes: o que entrou hoje já está no realizado, e hoje
já está no divisor do ritmo atual, então a multiplicação usa só os dias úteis
**depois** de hoje. Por isso, num dia útil, o quadro mostra um dia a menos que o
"Falta para bater". A conta equivale a ritmo atual × dias úteis do mês.

A projeção herda uma limitação do ritmo atual: enquanto a conciliação do dia
não chega, hoje entra no divisor com pouco ou nada no realizado, e a projeção
fica mais baixa até o lote do dia aparecer. E o custo não tem ritmo — é o valor
do mês inteiro, descontado de uma vez do faturamento projetado.

### Dia a dia

Na coluna da direita, um gráfico resume o mês numa imagem: uma coluna por dia
útil, o recebimento do dia na altura, e uma linha de referência na meta
dividida por igual pelos dias úteis. Passar o mouse — ou dar foco pelo teclado
— mostra o dia e o valor. Só o maior dia vem com o valor escrito: número em
cima de toda coluna vira ruído e ninguém lê.

Dia útil que passou sem nada é um traço fino; dia que ainda não chegou é um
traço mais apagado. As duas coisas não são a mesma, e juntas as colunas
apagadas mostram quanto de mês ainda falta. A mesma informação em tabela é a
tela de Histórico, então nada fica preso na dica.

O gráfico fica só no computador: a grade da TV foi medida para caber numa tela
sem rolagem, e um quadro a mais cortaria o resto.

### Feriados

Feriado não é dia útil: sai do ritmo atual, do ritmo necessário, da projeção e
da meta do dia — na TV, feriado fica sem meta do dia, como o fim de semana. O
painel desconta os feriados nacionais, o carnaval (segunda e terça) e Corpus
Christi. Os de data móvel saem da Páscoa, então o calendário vale para qualquer
ano sem manutenção.

A lista não foi chutada. Na conciliação de 2026, carnaval, Tiradentes, 1º de
maio, Corpus Christi e 7 de setembro não tiveram nenhuma receita, e a
Sexta-feira Santa teve uma só. A quarta-feira de cinzas teve dia normal e
continua útil.

Feriado municipal ou dia sem expediente entra em `FERIADOS_EXTRAS`, na
configuração, no formato `"AAAA-MM-DD"`.

### A receita não se edita pelo painel

O painel exibe recebimento; não o cria, não o corrige e não o apaga. Meta e
custo do mês continuam sendo digitados, porque não saem de lugar nenhum — mas
não são receita.

Isso já foi diferente, e o motivo de ter mudado vale registrar. Enquanto dava
para apagar uma linha pelo painel, o id dela ia para uma lista de *ocultos* no
navegador e era pulado **em toda leitura seguinte, para sempre**. Uma linha
corrigida pelo ✎ "descolava" da planilha e parava de ser atualizada. As duas
coisas moravam só naquele navegador: a tela dizia "2.734 receitas lidas da
planilha" e mostrava um total que a planilha nunca disse. Num painel pendurado
na parede, ninguém tinha como desconfiar.

Por isso a leitura passou a ser substituição, não mistura: o que está na tela é
o que está na planilha. E, ao abrir, o painel descarta o que tiver ficado
guardado de lançamento digitado, linha corrigida ou lista de ocultos — um
navegador que já tenha sido mexido se conserta sozinho no primeiro acesso.

## Modo TV

Acrescente `?tv` ao endereço — `https://vetorsst.github.io/painel-diario/?tv` — e
o painel vira tela de parede: sem campo de meta, sem custo, sem histórico. Só o
que se lê de longe. Sem o `?tv`, o mesmo arquivo abre o painel completo, com
meta, custo e histórico.

### O que a TV mostra

O número grande é a **meta do dia**, porque é o que a equipe consegue mudar
hoje. Abaixo dele, a barra do dia; depois, o mês contra a meta, quanto falta e
quantos recebimentos entraram hoje; depois, quem pagou.

A projeção do mês aparece na nota do bloco do mês, no lugar do valor da meta —
que continua no rodapé. Com o custo do mês na configuração, a nota passa a
trazer a projeção de faturamento e o lucro projetado. Nenhum dos dois ganhou
bloco próprio porque a grade de três foi medida para caber sem rolagem, e um
quarto bloco cortaria as notas.

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

A planilha de conciliação é a **única fonte de receita** do painel. Cada linha
com `Categoria = Receita` vira um lançamento, com o **nome de quem pagou** no
lugar do cliente. O painel lê e exibe; quem altera o número é a planilha.

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
| Linha apagada, corrigida ou digitada num navegador antigo | é descartada ao abrir |

O rodapé mostra quantas receitas foram lidas e a que horas — ou o motivo, se a
leitura falhar.

Há uma trava contra leitura estragada: se um lote novo fizer sumir mais de 30%
das linhas que vieram da planilha, o painel **recusa a leitura** e acende o
alerta em vez de aplicar. Um CSV cortado no meio do download, ou a `=QUERY`
virando `#REF!`, devolve um resultado válido e curto — e sem essa trava o mês
inteiro evaporaria da tela sem ninguém perceber.

### Meta e custo do mês

O quadro "Meta do mês" tem dois campos. A **meta** passa a valer no lugar do
`META_MES` da configuração, e dela saem o ritmo necessário, quanto falta e a
meta de hoje. O **custo** passa a valer no lugar do `CUSTO_MES`, e dele sai o
lucro projetado; deixado em branco, vale o da configuração.

O que se salva no quadro fica guardado naquele navegador. A TV, que é só
leitura, usa o que estiver na configuração — é lá que o custo precisa estar
para o lucro aparecer na parede.

## Configuração

A configuração fica no início da tag `<script>` do arquivo, em `var CONFIG`:

| Chave | O que faz |
| --- | --- |
| `META_MES` | Meta do mês, em reais (padrão: `250000`). O que for salvo no painel manda nisto. |
| `CUSTO_MES` | Custo total do mês, em reais (padrão: `0`, sem custo). Dele sai o lucro projetado. O que for salvo no painel manda nisto. |
| `FERIADOS_EXTRAS` | Dias sem expediente além dos feriados nacionais, carnaval e Corpus Christi, como `["2026-11-30"]`. |
| `API_URL` | Endpoint que sincroniza **meta e custo** entre navegadores. Vazio: cada navegador guarda os seus no `localStorage`. Receita nunca passa por aqui. |
| `API_TOKEN` | Opcional, enviado como `Authorization: Bearer ...`. |
| `SYNC_SEGUNDOS` | Intervalo de sincronização com a API (padrão: `15`). |
| `CSV_RECEITAS` | URL do CSV publicado da conciliação bancária. É a única fonte de receita do painel: vazio, o painel não tem o que mostrar. |
| `CSV_SEGUNDOS` | Intervalo de leitura da planilha (padrão: `300`). |

### Contrato da API

A API sincroniza **só meta e custo** entre navegadores. Receita não passa por
ela: vem da planilha financeira e de mais lugar nenhum, nem de um servidor.

Dois métodos na mesma URL:

- `GET` devolve `{"metaMes":250000,"custoMes":150000}`.
- `POST` recebe o mesmo corpo e grava.

`custoMes` pode vir `null`, e aí vale o `CUSTO_MES` da configuração.

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
