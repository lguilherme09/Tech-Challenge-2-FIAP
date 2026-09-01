# Notebooks

Execute nesta ordem — cada notebook consome os artefatos gerados pelo anterior:

| # | Notebook | O que faz |
|---|---|---|
| 1 | `01_eda.ipynb` | Compreensão do problema e análise exploratória de dados |
| 2 | `02_preprocessamento.ipynb` | Limpeza, feature engineering, split treino/teste e padronização — gera `data/processed/` |
| 3 | `03_modelagem.ipynb` | Treino dos modelos (KNN, Árvore de Decisão, Random Forest) — gera `data/processed/models/` |
| 4 | `04_avaliacao.ipynb` | Métricas de avaliação, importância de variáveis e conclusões |

**Semente fixa:** `RANDOM_STATE = 42`, declarada na primeira célula com código
de cada notebook. Rodar os notebooks na ordem acima, a partir de um ambiente
limpo (ver `README.md` na raiz), reproduz exatamente os números da seção
"Resultados" do README principal.
