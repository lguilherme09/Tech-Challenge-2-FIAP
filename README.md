# Classificando a Qualidade de Vinhos com Machine Learning

Tech Challenge — Fase 2 | Pós-Graduação **Data Analytics** (FIAP)

## Descrição do projeto

Este projeto desenvolve uma pipeline de análise e modelagem para prever se um
vinho é de **Alta Qualidade** (nota ≥ 7) ou de **Baixa/Média Qualidade**
(nota < 7), a partir de suas características físico-químicas (acidez, açúcar
residual, cloretos, dióxido de enxofre, densidade, pH, sulfatos e teor
alcoólico), utilizando o **Wine Quality Dataset**.

O problema foi tratado como uma **classificação binária**, seguindo as etapas:
compreensão do problema → análise exploratória de dados (EDA) →
pré-processamento → treino de modelos → avaliação → interpretação dos
resultados.

## Estrutura do repositório

```
wine-quality-classification/
│
├── data/                 # Base de dados utilizada (WineQT.csv)
├── notebooks/             # Notebook com a análise e modelagem completas
├── src/                    # Scripts auxiliares (reservado para uso futuro)
├── results/                # Gráficos e métricas exportados pelo notebook
├── requirements.txt        # Bibliotecas utilizadas
└── README.md                # Este arquivo
```

## Como executar

```bash
# 1. Criar/ativar um ambiente virtual (opcional, mas recomendado)
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate

# 2. Instalar as dependências
pip install -r requirements.txt

# 3. Abrir o notebook
jupyter notebook notebooks/wine_quality_classification.ipynb
```

O notebook lê a base diretamente de `../data/WineQT.csv` e salva todos os
gráficos e a tabela comparativa de métricas em `results/`.

## Modelos treinados

Foram treinados e comparados três modelos de classificação:

- KNN (K-Nearest Neighbors)
- Decision Tree
- Random Forest

Os modelos baseados em árvore usam `class_weight="balanced"` para compensar o
desbalanceamento de classes identificado na EDA (~14% de vinhos de alta
qualidade contra ~86% de baixa/média qualidade).

## Principais resultados

| Modelo             | Acurácia | Precisão (classe 1) | Recall (classe 1) | F1-score (classe 1) | AUC   |
|---------------------|----------|----------------------|--------------------|-----------------------|-------|
| KNN                  | 0.851    | 0.457                | 0.333              | 0.386                 | 0.766 |
| Decision Tree        | 0.790    | 0.360                | 0.646              | 0.463                 | 0.763 |
| Random Forest        | 0.892    | 0.720                | 0.375              | 0.493                 | 0.855 |

- O Random Forest teve o melhor desempenho geral (maior acurácia, AUC e precisão para a classe "Alta Qualidade").
- A Árvore de Decisão teve o maior *recall* — identifica mais vinhos de alta qualidade, mas com mais falsos positivos.
- As variáveis mais importantes para o modelo foram, em ordem:
  `alcohol`, `acid_ratio` (feature criada), `citric acid`, `sulphates` e
  `volatile acidity` — coerente com a análise de correlação feita na EDA.

Detalhes completos (gráficos, matrizes de confusão, curva ROC e discussão) estão
no notebook em `notebooks/wine_quality_classification.ipynb`.

## Limitações e próximos passos

- Base relativamente pequena (1.143 amostras) e desbalanceada, o que limita o recall dos modelos para a classe minoritária.
- Apenas variáveis físico-químicas estão disponíveis — fatores como safra, tipo de uva e processo de vinificação não estão representados.