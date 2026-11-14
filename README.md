# FieldShift — NASA Space Apps 2026

Protótipo para o desafio **Field Shift: Adapting Farms with NASA Data**: uma ferramenta de apoio à decisão que usa dados da
**NASA POWER**, o solo do local, as características das culturas e as prioridades do agricultor para sugerir **rotações de
culturas** que protejam o solo e se adaptem ao clima que está mudando.

## Notebook

[`FieldShift_NASA_POWER_risco.ipynb`](FieldShift_NASA_POWER_risco.ipynb) ([abrir no Colab](https://colab.research.google.com/github/izancnpereira-star/fieldshift-spaceapps/blob/main/FieldShift_NASA_POWER_risco.ipynb)):

1. Baixa a série diária da NASA POWER (chuva, temperatura máx./mín.) para um ponto (padrão: Uberlândia-MG).
2. Calcula a ETo (Hargreaves) e roda um balanço hídrico simplificado inspirado no SARRA/ZARC.
3. Calcula o ISNA e o **risco climático** de 6 culturas (soja, milho safrinha, sorgo, feijão, crotalária e braquiária)
   para 36 decêndios de semeadura × 6 níveis de retenção de água no solo (muito baixa → muito alta; classes AD1–AD6 do ZARC).
4. **Field Shift:** compara o risco entre 1991–2008 e 2009–2024.
5. **Recomendador de rotação:** compara pelo menos três estratégias, **sempre incluindo a rotação atual** do agricultor,
   em **cartões verde/amarelo/vermelho** (faixas de risco do ZARC). Cada cartão mostra risco de falta de água e quanto ele
   mudou, água que a lavoura usa, dias de calor extremo, risco de geada, por que foi sugerida, benefícios, o que avaliar
   com o agrônomo e onde faltam informações.
6. **Como ler os resultados:** glossário em linguagem simples (evapotranspiração, balanço hídrico, ISNA, decêndio) e o
   aviso de que a ferramenta apoia, mas não substitui, o produtor e o agrônomo.

## Como usar

1. Abra o notebook (no Colab ou no VS Code).
2. Edite só a célula **"Dados do produtor"**: local, histórico (ex.: `["soja", "milho_safrinha"]`), tipo de solo
   (`"arenoso"`, `"médio"`, `"argiloso"`, `"muito argiloso"` ou, com análise de solo, `"AD1"`…`"AD6"`),
   data de plantio e pesos das prioridades.
3. Rode tudo (*Run All*). O **Painel do produtor**, no fim, mostra a sugestão em linguagem simples, a tabela e o gráfico.

Saídas: `tabua_risco_nasa_power.csv` (risco por cultura, solo e decêndio) e `comparacao_periodos.csv` (mudança entre períodos).

Dependências: `requests`, `pandas`, `numpy`, `matplotlib`, `scipy`. A simulação é vetorizada e roda em segundos,
mesmo em computador com 4 GB de RAM.

> 📚 **Parâmetros:** ciclos, limites de ISNA, profundidade de raiz, água disponível no solo e limites de calor/geada vêm das
> portarias do **ZARC** (MAPA/Embrapa): soja MG 2025/26, milho 2ª safra DF, feijão 1ª safra MG e sorgo granífero.
> A divisão do ciclo em fases e o Kc seguem a FAO-56. Crotalária, braquiária e as notas de saúde do solo são **estimativas**
> a validar com agrônomo.