# FieldShift — NASA Space Apps

Protótipo de análise de risco climático de plantio com dados da **NASA POWER**.

## Notebook

[`FieldShift_NASA_POWER_risco.ipynb`](FieldShift_NASA_POWER_risco.ipynb) ([abrir no Colab](https://colab.research.google.com/github/izancnpereira-star/fieldshift-spaceapps/blob/main/FieldShift_NASA_POWER_risco.ipynb)):

1. Baixa a série diária da NASA POWER (chuva, temperatura máx./mín.) para um ponto (padrão: Uberlândia-MG).
2. Calcula a ETo (Hargreaves) e roda um balanço hídrico simplificado inspirado no SARRA/ZARC.
3. Calcula o ISNA por fase da soja e o risco para 36 decêndios de semeadura × 6 classes de água no solo (AD1–AD6).
4. **Field Shift:** compara o risco entre 1991–2008 e 2009–2024.

Dependências: `requests`, `pandas`, `numpy`, `matplotlib`, `scipy`.

> ⚠️ Parâmetros da cultura (fases, Kc) e classes AD5/AD6 são provisórios — calibrar com a documentação do ZARC.
