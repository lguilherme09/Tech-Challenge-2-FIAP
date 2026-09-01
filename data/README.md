# Dados

## Como obter o dataset

Este projeto usa o **Wine Quality Dataset**, disponível publicamente no Kaggle:

🔗 https://www.kaggle.com/datasets/yasserh/wine-quality-dataset

1. Baixe o arquivo `WineQT.csv` a partir do link acima (é necessário login no Kaggle).
2. Coloque o arquivo em `data/raw/WineQT.csv`.
3. Nenhum outro tratamento é necessário antes de rodar `notebooks/01_eda.ipynb` — a
   limpeza e o pré-processamento acontecem dentro dos notebooks.

O arquivo bruto **não é versionado neste repositório** (ver `.gitignore`), para
manter o repositório leve e respeitar os termos de uso da fonte.

## Estrutura desta pasta

```
data/
├── raw/          arquivo original baixado do Kaggle (WineQT.csv) — não versionado
└── processed/    gerado automaticamente pelo notebook 02_preprocessamento.ipynb
                  (train.csv, test.csv, scaler.pkl, models/*.pkl) — não versionado
```

## Dicionário de variáveis

| Variável | Descrição |
|---|---|
| fixed acidity | Acidez fixa (ácidos não voláteis) |
| volatile acidity | Acidez volátil (associada a defeitos de fermentação) |
| citric acid | Ácido cítrico |
| residual sugar | Açúcar residual após fermentação |
| chlorides | Cloretos (sal) |
| free sulfur dioxide | Dióxido de enxofre livre |
| total sulfur dioxide | Dióxido de enxofre total |
| density | Densidade |
| pH | Nível de acidez/basicidade |
| sulphates | Sulfatos (conservante) |
| alcohol | Teor alcoólico |
| quality | Nota de qualidade (3–8), atribuída por especialistas |
| Id | Identificador sequencial da amostra (removido antes da modelagem) |
