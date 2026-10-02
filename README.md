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

**Saíram desta versão:** APP, Reserva Legal e parcela SNCI pública.

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

Há botões **ⓘ** espalhados pelos controles explicando cada um. O botão
**Baixar PNG** salva o gráfico como está na tela.

No Sankey toda comparação é separada, por imposição do próprio gráfico: ele
precisa que o que sai de uma coluna seja exatamente o que entra na seguinte.

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
