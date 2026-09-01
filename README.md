# Tech Challenge — Fase 2 | POSTECH Data Analytics

## 1. Identificação

| Campo | Valor |
| --- | --- |
| Turma | Pós Tech - Data Analytics (14DTAT) |
| Grupo | Individual |
| Data de entrega | 01/09/2026 |

### Integrantes

| Nome completo | RM | E-mail |
| --- | --- | --- |
| Luis Guilherme Betarelli Lopes | RM374939 | rm374939@fiap.com.br / luisguilhermeblopes@gmail.com |

---

## 2. Links da entrega

Estes três links são **obrigatórios** e devem ser idênticos aos do PDF de submissão.

| Item | Link |
| --- | --- |
| Repositório | https://github.com/lguilherme09/Tech-Challenge-2-FIAP |
| Vídeo executivo (≤ 5 min) | https://drive.google.com/file/d/1n8SFZ7I9Z1GZib4R41MSNBt92XaMVWmk/view?usp=drive_link |
| Apresentação | https://docs.google.com/presentation/d/1LQ9jfwEfevdiR_62fzwVcfRdnidHqXqZ/view?usp=drive_link |

> ⚠️ **Atenção:** ao testar os dois links do Drive em janela anônima, o vídeo pediu login (erro 401) e a apresentação retornou "File could not open" — provavelmente o compartilhamento ainda não está como "Qualquer pessoa com o link". Ajuste a permissão para **Leitor** antes de submeter, e confirme abrindo os dois links em uma janela anônima.
> Repositório privado ou inacessível **zera** toda a Dimensão 1 da rúbrica.

---

## 3. O problema

A avaliação tradicional de qualidade de um vinho depende de especialistas realizando
análise sensorial (aroma, sabor, acidez, equilíbrio) — um processo subjetivo, caro
e difícil de padronizar em escala. Este projeto usa variáveis físico-químicas,
medidas durante a própria produção, para prever a qualidade final do vinho.

### Variável alvo

A nota de qualidade original (`quality`, de 3 a 8) foi transformada em uma
**classificação binária**, conforme definido no enunciado do desafio:

- **1 — Alta Qualidade**: `quality >= 7`
- **0 — Baixa/Média Qualidade**: `quality < 7`

### Dataset

| Campo | Valor |
| --- | --- |
| Fonte | [Wine Quality Dataset — Kaggle (yasserh)](https://www.kaggle.com/datasets/yasserh/wine-quality-dataset) |
| Linhas × colunas | 1.143 × 13 (12 após remover `Id`) |
| Período / versão | Publicado no Kaggle em jan/2022 (dataset original: Cortez et al., 2009) |
| Licença de uso | O dataset original (Cortez et al., 2009) é distribuído pelo UCI Machine Learning Repository sob licença **CC BY 4.0**. A página do Kaggle usada como fonte direta não exibe uma licença própria diferente — recomenda-se creditar a fonte original em qualquer uso. |

Descrição das variáveis:

| Variável | Tipo | Descrição |
| --- | --- | --- |
| fixed acidity | float | Acidez fixa (ácidos não voláteis) |
| volatile acidity | float | Acidez volátil (associada a defeitos de fermentação) |
| citric acid | float | Ácido cítrico |
| residual sugar | float | Açúcar residual após fermentação |
| chlorides | float | Cloretos (sal) |
| free sulfur dioxide | float | Dióxido de enxofre livre |
| total sulfur dioxide | float | Dióxido de enxofre total |
| density | float | Densidade |
| pH | float | Nível de acidez/basicidade |
| sulphates | float | Sulfatos (conservante) |
| alcohol | float | Teor alcoólico |
| quality | int | Nota de qualidade (3–8) — variável alvo original |
| Id | int | Identificador sequencial — removido antes da modelagem |

---

## 4. Como reproduzir

```bash
git clone https://github.com/lguilherme09/Tech-Challenge-2-FIAP.git
cd Tech-Challenge-2-FIAP

python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

pip install -r requirements.txt
jupyter notebook
```

Baixe o dataset e coloque o arquivo bruto em `data/raw/` (os dados **não** são
versionados — veja `data/README.md`).

Depois execute os notebooks nesta ordem:

| # | Notebook | O que faz |
| --- | --- | --- |
| 1 | `notebooks/01_eda.ipynb` | Análise exploratória |
| 2 | `notebooks/02_preprocessamento.ipynb` | Limpeza, escala e feature engineering |
| 3 | `notebooks/03_modelagem.ipynb` | Treino e comparação dos modelos |
| 4 | `notebooks/04_avaliacao.ipynb` | Métricas, importância de variáveis e conclusões |

**Semente fixa:** `RANDOM_STATE = 42`, declarada na primeira célula de cada notebook.
Rodar os notebooks na ordem acima, a partir de um ambiente limpo, reproduz
exatamente os números da seção 5.

---

## 5. Resultados

| Modelo | Acurácia | Precisão | Recall | F1 | AUC-ROC |
| --- | --- | --- | --- | --- | --- |
| KNN | 0.851 | 0.457 | 0.333 | 0.386 | 0.766 |
| Árvore de Decisão | 0.790 | 0.360 | 0.646 | 0.463 | 0.763 |
| Random Forest | 0.892 | 0.720 | 0.375 | 0.493 | 0.855 |

*(Precisão, Recall e F1 referem-se à classe "Alta Qualidade", a de maior interesse de negócio.)*

**Modelo escolhido:** Random Forest — melhor acurácia, melhor AUC-ROC e melhor
precisão para a classe "Alta Qualidade" entre os três modelos testados.

**Métricas priorizadas:** como a base é desbalanceada (~14% Alta Qualidade vs.
~86% Baixa/Média), acurácia isolada seria enganosa — um modelo que sempre
previsse "Baixa/Média Qualidade" já acertaria ~86% das vezes sem aprender nada
útil. Por isso, a escolha do modelo priorizou **AUC-ROC** e a
**precisão/recall da classe minoritária**, não apenas a acurácia geral.

---

## 6. Principais conclusões

1. **Teor alcoólico** e **acidez volátil** são os fatores físico-químicos mais
   associados à qualidade do vinho — quanto maior o álcool e menor a acidez
   volátil, maior a tendência de nota alta.
2. A feature criada **`acid_ratio`** (razão entre acidez fixa e volátil) ficou
   entre as variáveis mais importantes do modelo, validando a etapa de
   feature engineering.
3. Existe um trade-off claro entre os modelos: o **Random Forest** tem melhor
   precisão (72%) mas captura menos vinhos de alta qualidade (recall 38%); a
   **Árvore de Decisão** captura mais vinhos bons (recall 65%) às custas de
   mais falsos positivos. A escolha entre eles depende do custo de negócio de
   cada tipo de erro.
4. Do ponto de vista de produção, o controle do processo de **fermentação**
   (que determina teor alcoólico e acidez volátil) é o ponto de maior
   alavancagem para elevar a qualidade final.

### Limitações e próximos passos

- Base relativamente pequena (1.143 amostras) e desbalanceada, o que limita o
  recall dos modelos para a classe minoritária.
- Apenas variáveis físico-químicas estão disponíveis — fatores como safra,
  tipo de uva e processo de vinificação não estão representados.
- Próximos passos sugeridos: balanceamento de classes (ex. SMOTE), otimização
  de hiperparâmetros (`GridSearchCV`) e testes com outros algoritmos (ex.
  Gradient Boosting).

---

## 7. Estrutura do repositório

```
.
├── data/          dados brutos (raw) e tratados (processed) — não versionados
├── notebooks/     análise em ordem numerada
└── docs/          apresentação executiva
```

Detalhes e convenções em [`ESTRUTURA.md`](ESTRUTURA.md).
Antes de enviar, percorra o [`CHECKLIST.md`](CHECKLIST.md).

---

## 8. Tecnologias

- Python 3
- pandas, numpy — manipulação de dados
- matplotlib, seaborn — visualização
- scikit-learn — modelagem (KNN, Árvore de Decisão, Random Forest) e métricas
- scipy — apoio estatístico
- joblib — serialização dos modelos treinados
- Jupyter Notebook — desenvolvimento e documentação da análise
