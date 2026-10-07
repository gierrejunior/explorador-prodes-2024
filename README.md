# Explorador de combinações — PRODES 2024 a 10 metros

Página única, autocontida, para explorar o desmatamento apontado pelo PRODES em
2024 cruzado com CAR, base fundiária SIGEF/SNCI, categorias territoriais,
categoria de unidade de conservação, tamanho do imóvel em módulos fiscais e uso
do solo pelo MapBiomas — **na grade de 10 metros**, com recorte por estado.

**Abrir:** https://gierrejunior.github.io/explorador-prodes-2024/

Acrescente `?validar` ao endereço para a página conferir a si mesma: ela refaz
em JavaScript as combinações calculadas em pandas e mostra o placar.

## O que mudou em relação à versão anterior

| | antes (30 m) | agora (10 m) |
|---|---|---|
| resolução | 30 m | **10 m**, nove vezes mais pixels |
| uso do solo | MapBiomas Coleção 11 | **Coleção 4**, a única disponível a 10 m |
| CAR | inteiro | **filtrado**: imóvel rural, cadastro ativo ou pendente, não cancelado |
| aderência ao fundiário | medida por **parcela** | medida por **imóvel**, com as parcelas dissolvidas |
| unidade de conservação | esfera × grupo | **mais as 12 categorias do SNUC** |

**Saiu desta versão:** parcela SNCI pública. **APP e Reserva Legal** declaradas
no CAR voltaram, junto com a **sobreposição entre imóveis do CAR** — em duas
leituras que não se somam: a área sobreposta mede o chão; o imóvel com
sobreposição mede o imóvel inteiro.

O filtro do CAR mantém 8.268.132 de 8.349.966 imóveis, 99,0% dos registros — mas remove assentamentos e territórios tradicionais, que concentram desmatamento. É por isso que a parcela do desmatamento "dentro do CAR" cai em relação à versão anterior.

## Números principais

| | ha | % |
|---|---|---|
| **Desmatamento PRODES 2024** | **1.957.066** | 100% |
| Dentro de imóvel do CAR | 1.499.559 | 76,6% |
| Em alguma das doze categorias territoriais | 451.628 | 23,1% |
| Imóvel adere ao SIGEF (≥90% nos dois sentidos) | 457.159 | 23,4% |
| Imóvel adere ao SNCI | 89.237 | 4,6% |
| Tem parcela sobreposta, mas abaixo de 90% | 288.948 | 14,8% |
| Sem parcela fundiária | 796.301 | 40,7% |
| Imóvel pequeno (até 4 módulos fiscais) | 525.493 | 26,9% |
| Imóvel médio (mais de 4 e até 15) | 375.696 | 19,2% |
| Imóvel grande (mais de 15) | 709.319 | 36,2% |

As categorias se sobrepõem de propósito: o raster não embute regra de
atribuição, então somar linhas dá mais que o total.

### Desmatamento em unidade de conservação

As duas tabelas descrevem **as mesmas** unidades por dois critérios distintos —
a categoria do SNUC e a esfera administrativa combinada com o grupo de manejo —
e portanto não se somam entre si.

Por categoria do SNUC:

| categoria | ha |
|---|---|
| Área de Proteção Ambiental | 77.507 |
| Reserva Extrativista | 10.331 |
| Floresta | 7.180 |
| Parque | 4.492 |
| Estação Ecológica | 4.241 |
| Reserva de Desenvolvimento Sustentável | 2.095 |
| Reserva Biológica | 1.023 |
| Refúgio de Vida Silvestre | 815 |
| Reserva Particular do Patrimônio Natural | 173 |
| Monumento Natural | 161 |
| Área de Relevante Interesse Ecológico | 26 |

Por esfera e grupo de manejo (US = uso sustentável, PI = proteção integral):

| esfera e grupo | ha |
|---|---|
| Estadual + US | 58.417 |
| Federal + US | 36.467 |
| Estadual + PI | 6.685 |
| Federal + PI | 4.018 |
| Municipal + US | 2.865 |
| Municipal + PI | 30 |

## Por que a aderência caiu em relação à versão anterior

Medir a aderência por **imóvel** em vez de por **parcela** parecia só uma
correção: um imóvel do CAR composto de várias parcelas reprovava no critério
porque cada parcela cobria só uma fração dele.

A correção funcionou — **114.618 imóveis** passaram a aderir. Mas ela também
quebrou o caso oposto: ao fundir parcelas, o imóvel fundiário fica maior e passa
a cobrir **vários** imóveis do CAR, e aí reprova no sentido inverso. Foram
**262.711 imóveis** perdidos por isso.

O saldo é uma queda de **20,4%**. O critério de 90% **nos dois sentidos** não é
invariante à unidade de agregação: as duas medições são enviesadas, em direções
opostas. Quem comparar os números das duas versões vai ver essa diferença, e ela
é metodológica, não um erro de uma delas.

## Como ler

À esquerda você **filtra** o que entra na conta; acima do gráfico **separa** o
que entrou. Dentro de um mesmo grupo os filtros somam com **ou**; entre grupos,
com **e**. A frase acima dos números descreve sempre o que está na tela.

Cada comparação tem o próprio botão de leitura, com três estados, que decide
como contar um pixel que pertence a mais de uma classe daquela comparação:

| | o que você vê | soma |
|---|---|---|
| **cada combinação** | "Pequena + Grande" aparece separada de "Grande" — mostra QUAIS se sobrepõem | fecha com o total |
| **sobreposições juntas** | tudo que se sobrepõe vira a classe "Sobreposição" — mostra QUANTO há de conflito | fecha com o total |
| **total por classe** | "Grande" já inclui o que divide pixel com "Pequena" — mostra QUANTO há de cada classe | passa do total, de propósito |

A **ordem** das comparações decide o que é nível 1 e nível 2 (ou a primeira
coluna do Sankey). Para trocar, arraste pela alça **⠿** ao lado do número — com
mouse ou com o dedo — e solte em cima da comparação cujo lugar ela deve ocupar;
no teclado, as setas ↑ ↓ sobre a alça fazem o mesmo. Cada comparação leva junto
o próprio modo de leitura. Os números são os mesmos de ter escolhido as
comparações nessa ordem; destaque e filtros continuam valendo, e o link copiado
sai na ordem nova.

Há botões **ⓘ** espalhados pelos controles explicando cada um. O botão
**Baixar PNG** salva o gráfico como está na tela.

No Sankey toda comparação é separada, por imposição do próprio gráfico: ele
precisa que o que sai de uma coluna seja exatamente o que entra na seguinte.

O **uso do solo** abre agrupado nos oito grupos definidos pelo time; o botão
**Detalhado**, no filtro e na comparação de uso do solo, volta às classes do
MapBiomas. Trocar de nível não muda a conta: os hectares de um grupo são a soma
exata das suas classes.

| grupo | classes do MapBiomas (códigos) |
|---|---|
| Floresta | Formação Florestal, Floresta Alagável, Formação Savânica, Savana Alagada, Mangue, Restinga Arbórea (3, 6, 4, 7, 5, 49) |
| Vegetação herbácea | Formação Campestre, Formação Herbáceo Arbustiva, Campo Alagado e Área Pantanosa, Marisma, Restinga Herbácea ou Arbustiva, Apicum, Afloramento Rochoso (12, 77, 11, 84, 50, 32, 29) |
| Pastagem | Pastagem (15) |
| Agricultura anual | Lavoura Temporária (19) e as culturas temporárias da coleção 11 (39, 20, 40, 62, 41) |
| Agricultura perene | Lavoura Perene (36) e as culturas perenes da coleção 11 (46, 47, 35, 48) |
| Silvicultura | Silvicultura (9) |
| Mosaico de usos agro | Mosaico de Usos (21) |
| Outros | Praia, Duna e Areal, Área Urbanizada, Mineração, Usina Fotovoltaica, Parque Eólico, Outras Áreas não Vegetadas, Rio, Lago e Oceano, Aquicultura (23, 24, 30, 75, 91, 25, 33, 31) |

A grade de 10 m não separa as culturas: a agricultura vem só como Lavoura
Temporária e Lavoura Perene, por isso os códigos 19 e 36 entram nos grupos.
**Fora do MapBiomas** (código 0, sem classificação) fica à parte, com o
próprio nome. Links copiados antes desta opção abrem no detalhado, como eram.

Além de treemap, sunburst, pizza e Sankey, há três gráficos:

- **Barras** — uma barra por categoria da comparação 1, dividida pela
  comparação 2, em hectares ou em 100% (para comparar a composição). É o mais
  fácil de ler: comprimento se compara melhor que área ou ângulo.
- **Sobreposições** — cada coluna é uma combinação de categorias da
  comparação 1 (as bolinhas dizem quais estão juntas no mesmo pixel; a barra,
  quantos hectares). À esquerda, o total de cada categoria, contando a
  sobreposição.
- **% desmatado** — quanto de cada categoria da comparação 1 foi desmatado em
  2024: área desmatada ÷ área total da categoria, sempre sobre o Brasil
  inteiro, com os filtros escolhidos menos o de desmatamento. Aderência e
  tamanho não entram: só existem dentro do desmatamento e dariam 100% por
  construção.

A **calculadora de sobreposição** recebe categorias de qualquer grupo — inclusive
o **uso do solo**, no nível escolhido em Agrupado/Detalhado (duas classes de uso
do solo nunca se sobrepõem: cada pixel tem uma só) e mostra,
com os nomes escolhidos: a área ocupada por elas juntas (a união, cada hectare
uma vez — o número para citar), a área que é de todas ao mesmo tempo (a
interseção), a área que seria contada duas vezes se as áreas fossem somadas
separadamente, a área só delas sem outra categoria dos mesmos grupos, e como a
área ocupada se divide.

**Destacar e filtrar.** São duas coisas, cada uma com seu lugar:

- **Clique** destaca: acende a parte clicada e apaga o resto, sem mudar a
  página. Dá para destacar várias; o resultado aparece no **Em foco**, logo
  acima do gráfico.
- **Filtrar por isto** (botão do Em foco) ou **duplo clique** filtram: a página
  inteira (cartões, gráfico, tabela, calculadora) passa a ser só aquilo, com um
  zoom curto partindo da parte escolhida. O filtro vira um passo da **trilha**
  acima do gráfico — `Brasil inteiro › Em CAR e Floresta › CAR + Terra Indígena`
  —, com **← voltar** e cada passo clicável para voltar direto a ele; o destaque
  esvazia ao filtrar, para a mesma coisa nunca estar nos dois lugares. Ao
  voltar, a tela volta como estava, com o destaque de antes de filtrar (se o
  filtro foi por duplo clique, acende o que foi filtrado); isso também vai no
  link.

Cada passo acumula com E sobre os anteriores; dentro de um passo, nomes da
mesma comparação somam com OU. O filtro é pelo rótulo exato — "CAR + Terra
Indígena" filtra só essa combinação —; no uso do solo, ele estreita o filtro de
uso do solo da esquerda. As porcentagens do destaque dizem sobre o quê são:
"1,8% de “Em CAR e Floresta”" dentro de um filtro, "da seleção" fora dele. A
trilha entra no link copiado; "Limpar todos os filtros" também a tira. O zoom não
muda o PNG e some para quem pede "reduzir movimento" no sistema.

**Destacar** segue a mesma regra de acumular: destacar "Sem parcela nenhuma" e
"Pastagem" acende só a Pastagem que está em Sem parcela e dá a conta desse
caminho (294.630 ha no desmatamento de 2024). No Sankey de três ou mais colunas
o destaque atravessa todas elas: o fluxo do Sankey só sabe o par de colunas
vizinhas, então a parte de cada fluxo que pertence ao caminho é recalculada nas
linhas e desenhada por cima, com a largura certa.

**Foco.** Clicar numa parte do gráfico ou num item da legenda destaca aquele
nome e apaga o resto: nas barras, a mesma categoria acende em todas as barras;
no Sankey, só os fluxos que saem ou chegam ao nó; no gráfico de sobreposições,
as colunas que contêm a categoria. Clicando em mais de um, todos ficam em foco.
Acima do gráfico aparecem o total de cada um e a área das escolhidas **juntas**
(cada hectare uma vez). A sobreposição aparece sempre, também quando é zero
("Sobreposição: 0 ha"), para não se confundir com conta que não foi feita; a
conta inteira fica em "ver o total de cada um": soma simples − sobreposição =
juntas; com duas, também "só A · nas duas · só B".

**Tabela e destaque são a mesma coisa.** Clicar numa linha da tabela põe o nome
no Em foco (o gráfico acende), e destacar no gráfico marca a linha (✓). A linha
"Σ em foco" traz o mesmo número do Em foco — cada hectare uma vez, mesmo com a
comparação em "total por classe", em que as linhas se sobrepõem — e embaixo dela,
a sobreposição. Como faz parte do foco, as linhas marcadas entram no link
copiado. Clicar no Σ limpa. Com a comparação em "cada combinação" e um eixo que
admite mais de uma categoria por pixel, a tabela diz também quando a
sobreposição é 0 ha.
A conta é feita direto nos dados, como a da calculadora, então vale também com
comparação em "somado". Os números mostrados fecham entre si: a soma é a dos
valores arredondados de cada um. Passar o
mouse na legenda mostra uma prévia; Esc ou "limpar foco" desfaz. O foco entra
no link copiado e no PNG: com foco ligado, a imagem sai como a tela (o resto
apagado) e com um rodapé "Em foco" que traz o total de cada item e o "Juntas";
sem foco, o PNG é o gráfico inteiro, como sempre.

No tema escuro a moldura ganha um brilho leve (bordas, cartões, botão ativo) e o
que está em foco brilha; no claro, vira uma sombra discreta. As cores dos dados
não mudam. Ao trocar filtro ou gráfico, cada gráfico entra do seu jeito: as
barras crescem; no Sankey os nós crescem e os fluxos se desenham da esquerda
para a direita, coluna por coluna; sunburst e pizza se revelam num giro a partir
do topo; o treemap surge com um zoom curto. Os números aparecem só no fim, já
com o valor final; quem pede "reduzir movimento" no sistema não vê animação.

**Cores.** Cada categoria tem cor fixa: a mesma em qualquer filtro, estado ou
gráfico. O uso do solo usa sempre as cores do MapBiomas — o verde fica só para
vegetação; o vermelho-alaranjado, só para o desmatamento; ausência ("Nenhum",
"Não avaliado", "Fora de UC") é cinza claro. As demais categorias usam seis
cores sem verde e sem vermelho, escolhidas para continuarem distintas entre si
em qualquer par para quem tem daltonismo; onde há mais categorias que cores, a
mesma cor em tons diferentes marca o que é do mesmo tipo (UC de proteção
integral, UC de uso sustentável, assentamentos). Uma combinação, como
"CAR + Terra Indígena", aparece listrada com as cores das categorias que a
formam.

O seletor **Tema**, no alto da página, escolhe claro, escuro ou automático — o
automático segue o modo claro ou escuro do computador ou do celular.

## Método

Grade de **10 m**, atribuição pelo **centro do pixel**. Área
calculada por linha de latitude em EPSG:6933 — não por área fixa de pixel, que
erraria −17,4% no extremo sul.

A aderência exige **90% nos dois sentidos**: o imóvel do CAR coberto pela
parcela certificada e a parcela coberta pelo imóvel. Só assim as duas geometrias
descrevem o mesmo chão.

O CAR é autodeclaratório e tem imóveis sobrepostos entre si — **86,5 milhões de
hectares** de sobreposição no país. Somar área de imóvel conta esse chão mais de
uma vez; o raster não, porque conta o pixel uma vez.

Os bits de **aderência e tamanho só existem para imóvel que cruza o desmatamento
de 2024**. Fora da mancha eles estão apagados por construção, não por ausência
de aderência.

O ano PRODES vai de agosto a julho, não coincide com o ano civil.

A cobertura do solo vem da **Coleção 4** do MapBiomas, a única disponível a
10 metros. A versão de 30 m usava a Coleção 11 — a legenda de códigos é a mesma,
mas são produtos diferentes e a classe de um pixel pode divergir entre elas.

---

Análise do projeto NORAD. Dados: PRODES/INPE, SICAR, SIGEF e SNCI/INCRA,
MapBiomas Coleção 4, CNUC, FUNAI, IBGE.
