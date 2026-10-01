# Explorador de combinações — PRODES 2024

Página única, autocontida, para explorar o desmatamento apontado pelo PRODES
em 2024 cruzado com CAR, base fundiária SIGEF/SNCI, categorias territoriais,
tamanho do imóvel em módulos fiscais e uso do solo pelo MapBiomas Coleção 11,
na grade de 30 m — com recorte por estado.

**Abrir:** https://gierrejunior.github.io/explorador-prodes-2024/

Acrescente `?validar` ao endereço para a página conferir a si mesma: ela refaz
em JavaScript 172 combinações calculadas em pandas e mostra o placar.

## Números principais

| | ha | % |
|---|---|---|
| Desmatamento PRODES 2024 | 1.957.042 | 100% |
| Em alguma das cinco categorias territoriais | 451.560 | 23,1% |
| Em CAR, não conforme ao fundiário | 1.206.234 | 61,6% |
| Em CAR, conforme (aderência ≥90%) | 605.940 | 31,0% |
| Fora do CAR | 262.653 | 13,4% |

Pastagem e Mosaico de Usos somam 78,7% do uso do solo apontado pelo MapBiomas
de 2025 nessa área.

## Como ler

À esquerda você **filtra** o que entra na conta; acima do gráfico **separa** o
que entrou. Dentro de um mesmo grupo os filtros somam com **ou**; entre grupos,
com **e**. A frase acima dos números descreve sempre o que está na tela.

O controle **Separado / Somado** decide como contar um pixel que pertence a mais
de uma classe. Separado: cada pixel numa classe só, as sobreposições viram classe
própria, a soma fecha com o total. Somado: o pixel conta em todas a que pertence,
e a soma passa do total de propósito.

## Método

Área calculada por linha de latitude em EPSG:6933 — não 900 m² fixos, que
erraria −17,4% no extremo sul. Atribuição pelo centro do pixel; o erro da
rasterização contra o vetor do PRODES é de −0,0004%.

Os sinalizadores se sobrepõem de propósito: o raster não embute regra de
atribuição, então somar categorias dá mais que o total. O CAR é autodeclaratório
e tem imóveis sobrepostos.

O ano PRODES vai de agosto a julho, não coincide com o ano civil.

---

Análise do projeto NORAD. Dados: PRODES/INPE, SICAR, SIGEF e SNCI/INCRA,
MapBiomas Coleção 11, IBGE.
