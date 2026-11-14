# FieldShift — NASA Space Apps 2026

Protótipo para o desafio **Field Shift: Adapting Farms with NASA Data**: uma ferramenta de apoio à decisão que usa dados de
observação da Terra da **NASA (NASA POWER)**, o solo do local, as características das culturas e as prioridades do agricultor
para comparar **rotações de culturas** que protejam o solo e se adaptem ao clima que está mudando.

> Esta ferramenta ajuda a enxergar padrões e opções. Ela **não substitui** a experiência do produtor nem a orientação de um
> agrônomo da região.

## Notebook

[`FieldShift_NASA_POWER_risco.ipynb`](FieldShift_NASA_POWER_risco.ipynb) —
[abrir no Colab](https://colab.research.google.com/github/izancnpereira-star/fieldshift-spaceapps/blob/feature/estrategias-cartoes/FieldShift_NASA_POWER_risco.ipynb)

1. **Dados da NASA:** baixa a série diária da NASA POWER (chuva, temperatura máxima e mínima, 1991–2025) para um ponto
   (padrão: Uberlândia-MG).
2. **Balanço hídrico:** calcula a evapotranspiração (Hargreaves) e roda um balanço hídrico tipo "balde", inspirado no
   modelo SARRA usado pelo ZARC.
3. **Risco climático:** calcula o ISNA e o risco de falta de água de 7 culturas (soja, soja precoce, milho safrinha, sorgo,
   feijão, crotalária e braquiária) para 36 decêndios de semeadura × 6 níveis de retenção de água no solo (classes AD1–AD6).
4. **Field Shift:** compara o risco entre 1991–2008 e 2009–2024 e testa a tendência da chuva de outubro a março (Mann-Kendall).
5. **Recomendador de rotação:** compara pelo menos três estratégias *safra de grão → segundo cultivo*, **sempre incluindo a
   rotação atual** do agricultor, em **cartões verde/amarelo/vermelho** (faixas de risco do ZARC, com ✓ ! ✕ além da cor).
   Cada cartão mostra:
   - risco de falta de água e quanto ele mudou desde 1991–2008;
   - água que a lavoura usa no ciclo, dias de calor extremo e risco de geada (limites do ZARC de cada cultura);
   - por que foi sugerida, benefícios, o que avaliar com o agrônomo e onde faltam informações.
6. **Quando plantar a soja:** tabela com o risco de soja → milho safrinha conforme a data de plantio da soja (normal ou
   precoce), mostrando quando o milho ainda entra na janela segura.
7. **Como ler os resultados:** glossário em linguagem simples (evapotranspiração, balanço hídrico, ISNA, decêndio) e a
   diferença entre dado de satélite e medição na fazenda.

## Como usar

1. Abra o notebook no Colab (link acima) ou no VS Code.
2. Edite só a célula **"Dados do produtor"**: local, histórico (ex.: `["soja", "milho_safrinha"]`), tipo de solo
   (`"arenoso"`, `"médio"`, `"argiloso"`, `"muito argiloso"` ou, com análise de solo, `"AD1"`…`"AD6"`),
   data de plantio e pesos das prioridades (risco climático, saúde do solo, diversificação).
3. Rode tudo (*Run All*). O **Painel do produtor**, no fim, mostra os cartões, o gráfico das notas e a tabela
   "quando plantar a soja".

Saídas: `tabua_risco_nasa_power.csv` (risco por cultura, solo e decêndio) e `comparacao_periodos.csv` (mudança entre períodos).

## Dados e fontes

| Fonte | O que usamos |
|---|---|
| [NASA POWER](https://power.larc.nasa.gov/) (API diária, comunidade AG) | Chuva corrigida (`PRECTOTCORR`), temperatura máxima e mínima a 2 m (`T2M_MAX`, `T2M_MIN`) |
| Portarias do **ZARC** (MAPA/Embrapa): [soja MG 2025/26](https://www.gov.br/agricultura/pt-br/assuntos/riscos-seguro/programa-nacional-de-zoneamento-agricola-de-risco-climatico/portarias/safra-vigente/minas-gerais/PORTN370SOJAMG.ret2.pdf), [milho 2ª safra DF](https://www.gov.br/agricultura/pt-br/assuntos/riscos-seguro/programa-nacional-de-zoneamento-agricola-de-risco-climatico/portarias/safra-vigente/distrito-federal/PORTN311MILHO2SAFRADF.reti.pdf), [feijão 1ª safra MG](https://www.gov.br/agricultura/pt-br/assuntos/riscos-seguro/programa-nacional-de-zoneamento-agricola-de-risco-climatico/portarias/safra-vigente/minas-gerais/PORTN18FEIJO1SAFRAMG.pdf), [sorgo granífero](https://www.gov.br/agricultura/pt-br/assuntos/riscos-seguro/programa-nacional-de-zoneamento-agricola-de-risco-climatico/portarias/2025-2026/rio-de-janeiro-rj/PORTN133SORGOGRANIFERORJ.pdf) | Ciclos, limites de ISNA, profundidade de raiz, água disponível no solo, limites de calor e geada, faixas de risco (20/30/40%) |
| IN SPA/MAPA nº 1/2022 | Classes de água disponível do solo AD1–AD6 |
| FAO-56 | Radiação extraterrestre, Hargreaves e coeficientes de cultura (Kc) |

## Ferramentas

Python (`requests`, `pandas`, `numpy`, `matplotlib`, `scipy`), Jupyter/Google Colab e Git/GitHub.
A simulação é vetorizada e roda em segundos, mesmo em computador com 4 GB de RAM.

## Limitações

- As portarias do ZARC não trazem a duração de cada fase nem o Kc: dividimos o ciclo em fases e usamos Kc da FAO-56.
- Crotalária e braquiária não têm ZARC; seus parâmetros e as notas de saúde do solo são **estimativas** a validar com agrônomo.
- Não considera pragas, doenças, preços, máquinas nem disponibilidade de semente.
- O NASA POWER é uma grade de cerca de 50 km: pode diferir do que acontece numa lavoura específica.
- Quando o solo é informado pelo tipo (arenoso, argiloso…), a classe de água é uma aproximação.
- Ainda falta validar o risco contra a [tábua de risco oficial do ZARC](https://dados.agricultura.gov.br/vi/dataset/tabua-de-risco-zoneamento-agricola-de-risco-climatico/resource/f9d597f9-0fee-47eb-9344-8642274ca9da).
