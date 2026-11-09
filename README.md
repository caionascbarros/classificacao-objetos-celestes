# Classificação de Objetos Celestes com Árvore de Decisão

Projeto final do **Dojo de Machine Learning do Hype USP**, grupo de extensão e estudos da Universidade de São Paulo. A entrega deste projeto marcou a conclusão do Dojo e a aceitação oficial como membro do grupo.

O objetivo é construir um modelo que classifique objetos observados pelo telescópio do **SDSS (Sloan Digital Sky Survey)** em três classes: **galáxia** (`GALAXY`), **estrela** (`STAR`) e **quasar** (`QSO`).

## Dataset

[Stellar Classification Dataset – SDSS17](https://www.kaggle.com/datasets/fedesoriano/stellar-classification-dataset-sdss17), disponível no Kaggle.

- 100.000 observações e 17 colunas
- Classes moderadamente desbalanceadas: `GALAXY` (~59%), `STAR` (~22%) e `QSO` (~19%)

O dataset **não está incluído no repositório**: o notebook baixa os dados automaticamente com a biblioteca `kagglehub`.

## Etapas do projeto

1. **Exploração dos dados**: estrutura, estatísticas descritivas e distribuição das classes
2. **Limpeza**: remoção de linhas com valores sentinela (`-9999`) nas colunas `u`, `g` e `z`
3. **Análise exploratória**: distribuições univariadas por classe, análise bivariada e matriz de correlação
4. **Seleção de variáveis**: escolha das features com base na análise, descartando identificadores e variáveis sem poder discriminativo
5. **Modelagem**: Árvore de Decisão (`DecisionTreeClassifier`), escolhida pela interpretabilidade
6. **Validação cruzada estratificada**: `StratifiedKFold` com 5 folds
7. **Ajuste de hiperparâmetros**: `GridSearchCV` sobre `criterion`, `max_depth` e `min_samples_leaf`
8. **Avaliação final**: métricas por classe no conjunto de teste, importância das variáveis e visualização da árvore com `graphviz`

## Resultados

| Modelo | Acurácia |
|---|---|
| Árvore de Decisão sem ajuste (teste) | 96,92% |
| Validação cruzada, 5 folds (modelo sem ajuste) | 96,67% ± 0,05% |
| Melhor modelo no GridSearchCV (média da validação cruzada) | 97,56% |
| **Modelo final ajustado (teste)** | **97,78%** |

Melhores hiperparâmetros: `criterion='entropy'`, `max_depth=None`, `min_samples_leaf=20`.

Desempenho do modelo final por classe no conjunto de teste:

| Classe | Precisão | Recall | F1-score |
|---|---|---|---|
| GALAXY | 0,98 | 0,99 | 0,98 |
| QSO | 0,96 | 0,93 | 0,94 |
| STAR | 1,00 | 1,00 | 1,00 |

### Principais conclusões

- O **`redshift`** é de longe a variável mais importante (~89% da importância no modelo final). Faz sentido fisicamente: o redshift está ligado à distância do objeto, o que separa estrelas (próximas) de galáxias e quasares (muito distantes).
- As bandas fotométricas `r` e `i` são altamente correlacionadas (~0,96), ou seja, carregam informação redundante.
- Por causa do desbalanceamento, o modelo foi avaliado com métricas por classe, e não apenas pela acurácia geral. A classe `QSO` é a mais difícil de classificar.

## Como executar

1. Clone o repositório:
   ```bash
   git clone https://github.com/caionascbarros/classificacao-objetos-celestes.git
   cd classificacao-objetos-celestes
   ```
2. Instale as dependências Python:
   ```bash
   pip install -r requirements.txt
   ```
3. Para a visualização da árvore, instale também o programa **Graphviz** no sistema (o pacote Python sozinho não basta):
   - Ubuntu/Debian: `sudo apt install graphviz`
   - macOS: `brew install graphviz`
   - Windows: baixe o instalador em [graphviz.org](https://graphviz.org/download/)
4. Abra e execute o notebook:
   ```bash
   jupyter notebook projeto_final_ml.ipynb
   ```

Também é possível rodar no Google Colab, que já vem com a maior parte das bibliotecas e com o Graphviz instalados.

## Tecnologias

Python · pandas · NumPy · Matplotlib · Seaborn · scikit-learn · kagglehub · Graphviz

## Autor

**Caio Barros**, estudante de Sistemas de Informação na USP e membro do Hype USP.
