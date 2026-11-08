# CLAUDE.md — contexto para o assistente

> O usuário chama o assistente de **Elo**. Responder sempre em **português**.
> Este repositório pode ser público: **não** registrar aqui dados de empresa (CNPJs, nomes de clientes,
> layouts internos), dados pessoais ou de saúde, nem chaves/segredos.

## Quem é o usuário

- **Izan** — Uberlândia-MG. Estudante de Ciência de Dados e Estatística na **UFU**.
- Trabalha com **engenharia de dados no crédito agro** (pipelines regulatórios de registro de recebíveis).
- Interesses: risco climático agrícola, risco de crédito, ML (XGBoost, validação cruzada),
  imputação de dados (KNN × MICE), otimização em grafos, estatística aplicada (CEQ, regressão, ANOVA).
- Participou do **NASA Space Apps** (2025: `Nasa1.ipynb`; 2026: este projeto).
- Desafios/extensão: REME (UFU), curso de SQL na UFU (PostgreSQL + SQL Murder Mystery).

## Como ele gosta de trabalhar

- Entender o problema antes de codar; código **enxuto, legível e modular**; foco em performance.
- Prefere **Polars** (incl. `scan_csv` lazy para arquivos grandes) e **pandas vetorizado**; prototipa no **Google Colab**.
- Gosta de **code review antes de commit**, READMEs bem feitos e Git Flow (feature branches).
- Mensagens curtas e diretas; explicar trade-offs de forma objetiva.

## O projeto: FieldShift

Objetivo: mostrar como o **risco climático de plantio** mudou ao longo do tempo (“field shift”) usando dados da NASA,
e a longo prazo alimentar um **Geo-Climatic Risk Rating** para crédito agrícola (ligado ao TCC dele).

- `FieldShift_NASA_POWER_risco.ipynb` (origem: Google Drive/Colab):
  1. Baixa série diária da **NASA POWER** (PRECTOTCORR, T2M_MAX, T2M_MIN) — ponto padrão Uberlândia (-18.9186, -48.2772).
  2. ETo por **Hargreaves** + balanço hídrico tipo balde (inspirado no SARRA/ZARC).
  3. **ISNA** por fase da soja para 36 decêndios × classes de solo AD1–AD6; risco = frequência de anos com falha
     (limites 0,50 fase inicial / 0,55 reprodutiva).
  4. Compara 1991–2008 × 2009–2024 e testa tendência da chuva out–mar (Kendall).
- Parâmetros da cultura (fases, Kc) e AD5/AD6 são **provisórios** — calibrar com a documentação do ZARC.

### Dados conhecidos

- `siszarc_cronograma.zip` (Drive, ~132 MB — grande demais para o GitHub): é o **cadastro de cultivares indicadas**,
  não a tábua de risco. Colunas: `Safra, Cultura, Regiao_de_Adaptacao, UF, Obtentor_Mantenedor, Grupo, Cultivar, nr_rnc`.
  Útil para o motor de rotação (cultivar/grupo por UF), **não** para validar o risco.
- Para validar o risco é preciso a **tábua de risco do ZARC** (dados abertos do MAPA) — ainda não obtida.

### Pendências / próximos passos

- [ ] Obter a tábua de risco ZARC (soja, Uberlândia) e comparar com `tabua_risco_nasa_power.csv`.
- [ ] Calibrar fases/Kc da soja; criar milho safrinha, sorgo, feijão.
- [ ] Rodar para vários municípios (lista lat/lon) e montar mapa.
- [ ] Motor de rotação (soja → milho safrinha → cobertura) usando janelas de baixo risco + tabela de cultivares.
- [ ] Ajustar o link do Colab no README quando existir a branch `main`.

## Notas técnicas do ambiente

- Notebooks do Colab vêm do Drive via base64; arquivos grandes (> ~100 KB) não cabem na conversa.
- GitHub recusa arquivos > 100 MB (usar recorte/Release/LFS).
