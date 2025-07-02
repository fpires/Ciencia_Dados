# PUC
# Análise e Modelagem Preditiva de Risco de Obesidade

Este projeto explora um dataset de indivíduos para prever e classificar diferentes níveis de obesidade com base em características como idade, altura, peso, hábitos alimentares e estilo de vida. O objetivo é aplicar técnicas de análise de dados, pré-processamento e modelos de Machine Learning para identificar padrões e construir modelos preditivos eficazes.

## Sumário

* [Visão Geral do Projeto](#visão-geral-do-projeto)
* [O Dataset](#o-dataset)
* [Pré-processamento de Dados](#pré-processamento-de-dados)
* [Análise Exploratória de Dados (EDA)](#análise-exploratória-de-dados-eda)
* [Modelagem Preditiva](#modelagem-preditiva)
* [Resultados e Conclusões](#resultados-e-conclusões)
* [Como Executar o Projeto](#como-executar-o-projeto)
* [Tecnologias Utilizadas](#tecnologias-utilizadas)

## Visão Geral do Projeto

O projeto aborda o problema da obesidade, uma condição de saúde global crescente, através da análise de um dataset contendo diversas características relacionadas aos hábitos e medidas corporais de indivíduos. Desenvolvemos modelos de Machine Learning para:
1.  **Classificar** indivíduos em diferentes níveis de obesidade (e.g., Peso Normal, Sobrepeso, Obesidade Tipo I, etc.).
2.  **Prever** o Índice de Massa Corporal (IMC) como uma variável numérica.

## O Dataset

O dataset utilizado, **"ObesityDataSet_raw_and_data_sinthetic.csv"**, contém informações sobre o risco de obesidade. As principais características incluem:

* **Características Numéricas:** `age`, `height`, `weight`, `fcvc` (frequência de consumo de vegetais), `ncp` (número de refeições principais), `ch2o` (consumo de água), `faf` (frequência de atividade física), `tue` (tempo de uso de dispositivos tecnológicos).
* **Características Categóricas/Binárias:** `gender`, `family_history_with_overweight` (histórico familiar de sobrepeso), `favc` (consumo frequente de alimentos de alto teor calórico), `scc` (monitoramento de calorias), `caec` (consumo de alimentos entre refeições), `mtrans` (meio de transporte).
* **Variável Alvo (Classificação):** `nobeyesdad` (Nível de Obesidade) - classes como `Insufficient_Weight`, `Normal_Weight`, `Obesity_Type_I`, etc.
* **Variável Alvo (Regressão):** `bmi` (Índice de Massa Corporal), uma variável calculada durante o pré-processamento.

## Pré-processamento de Dados

As seguintes etapas de pré-processamento foram aplicadas para preparar o dataset:

* **Padronização dos Nomes das Colunas:** Todos os nomes das colunas foram convertidos para minúsculas e padronizados.
* **Remoção de Duplicatas:** Foram identificadas e removidas 24 linhas duplicadas, resultando em um dataset limpo de 2087 entradas.
* **Mapeamento de Colunas Binárias:** Colunas como `family_history_with_overweight`, `favc`, `scc`, `smoke` foram mapeadas de strings ('yes'/'no') para valores numéricos (1/0).
* **Mapeamento de Colunas Ordinais:** Colunas como `calc` e `ch2o` (se aplicável) foram mapeadas de strings ordinais para numéricos.
* **Cálculo e Tratamento de `BMI`:** A coluna `bmi` foi calculada (`weight / (height^2)`) e valores `NaN` resultantes foram tratados.
* **Tratamento de `NaNs`:** Verificação final confirmou a ausência de valores ausentes.
* **One-Hot Encoding:** Colunas categóricas restantes (`gender`, `caec`, `mtrans`) foram transformadas usando One-Hot Encoding para criar novas colunas binárias. Após esta etapa, o dataset passou a ter 25 colunas.
* **Padronização (StandardScaler):** Todas as características numéricas (incluindo as geradas pelo One-Hot Encoding) foram padronizadas para ter média zero e desvio padrão um. Após esta etapa, o dataset transformado possui **2087 linhas e 25 colunas**.
* **Codificação da Variável Alvo:** A variável `nobeyesdad` (Nível de Obesidade) foi codificada numericamente para fins de classificação, mapeando as classes string para inteiros (e.g., 'Normal_Weight' -> 1).

## Análise Exploratória de Dados (EDA)

A EDA revelou insights importantes sobre o dataset:

* **Distribuição da Idade:** A maioria dos indivíduos no dataset está na faixa dos 20 anos, com a distribuição apresentando assimetria positiva.
* **Desvio Padrão dos Atributos:** O atributo `Weight` apresenta o maior desvio padrão (aprox. 26.19 kg), indicando a maior dispersão de dados, o que é esperado em um estudo sobre obesidade.
* **Média do Peso por Nível de Obesidade:** Há uma clara tendência de aumento do peso médio à medida que o nível de obesidade avança, demonstrando a relação direta entre as variáveis.
* **IMC por Nível de Obesidade:** Boxplots mostraram distribuições distintas de IMC para diferentes níveis de obesidade, validando a capacidade de separação das classes com base nesta métrica.
* **Consumo de Alimentos de Alto Teor Calórico por Idade:** A análise revelou diferenças na distribuição de idade entre aqueles que consomem e não consomem frequentemente alimentos de alto teor calórico, com uma tendência de os consumidores serem mais jovens.
* **Matriz de Correlação:**
    * Forte correlação positiva entre `weight` e `bmi` (0.93), o que é esperado, dado que o IMC é derivado do peso.
    * Correlação moderada entre `height` e `weight` (0.46).
    * Outras correlações entre `faf` e `tue` (atividade física e tempo de tela) também foram observadas, revelando padrões de estilo de vida.

## Modelagem Preditiva

Dois tipos de modelos foram aplicados:

### 1. Classificação (para prever o Nível de Obesidade)
* **Modelo:** Árvore de Decisão
* **Avaliação:** Validação Cruzada K-Fold (5 folds)

    **Métricas de Avaliação:**
    * **Acurácias por fold:** `[0.99 1.00 0.99 0.99 0.99]`
    * **Acurácia Média:** `0.99 (+/- 0.00)`
    * **Matriz de Confusão:** Demonstra excelente desempenho, com a maioria das classificações corretas na diagonal principal e pouquíssimos erros. Apenas a classe `Overweight_Level_I` teve um recall ligeiramente menor (0.86), embora com precisão perfeita (1.00).
    * **Relatório de Classificação:** Confirma alta precisão, recall e f1-score (>0.90 para a maioria das classes), com um desempenho notável para `Obesity_Type_III` (1.00 precisão, 0.99 recall).

### 2. Regressão (para prever o BMI)
* **Modelo:** Regressão Linear
* **Avaliação:** Validação Cruzada K-Fold (5 folds)

    **Métricas de Avaliação:**
    * **MAE (Erro Absoluto Médio):** `4.59` (em média, a previsão de BMI erra por 4.59 unidades).
    * **R² por fold:** `[0.50 0.48 0.50 0.42 0.49]`
    * **R² Médio:** `0.48 (+/- 0.03)` (o modelo explica 48% da variância do BMI, com pequena variação entre os folds).
    * **Gráfico de Previsões vs. Reais:** Mostra uma dispersão dos pontos em torno da linha ideal (y=x), indicando que o modelo captura a tendência, mas com uma precisão moderada.

## Resultados e Conclusões

* **Classificação (Nível de Obesidade):** O modelo **Árvore de Decisão** demonstrou um desempenho **excepcional** na classificação dos níveis de obesidade, atingindo uma **acurácia média de 99%** com altíssima estabilidade. A capacidade de distinguir entre as diferentes categorias é muito alta.
* **Regressão (Previsão de BMI):** O modelo de **Regressão Linear** para prever o IMC apresentou um desempenho **moderado**, explicando cerca de 48% da variabilidade. Embora capture a tendência geral, há espaço para melhorias na precisão das previsões de IMC, possivelmente através de modelos mais complexos ou mais engenharia de features.
* A análise exploratória foi crucial para entender as relações entre as variáveis e a importância de fatores como peso e IMC na determinação dos níveis de obesidade.

## Como Executar o Projeto

1.  **Clone o repositório:**
    ```bash
    git clone [https://github.com/seu-usuario/seu-repositorio.git](https://github.com/seu-usuario/seu-repositorio.git)
    cd seu-repositorio
    ```
2.  **Instale as dependências:**
    ```bash
    pip install pandas matplotlib seaborn scikit-learn numpy
    ```
3.  **Execute o notebook/script:**
    Abra o arquivo `.ipynb` (Jupyter Notebook) ou `.py` (script Python) e execute as células/código em sequência para replicar a análise e os modelos.

## Tecnologias Utilizadas

* Python
* Pandas (para manipulação e análise de dados)
* NumPy (para operações numéricas)
* Matplotlib (para visualização de dados)
* Seaborn (para visualização de dados estatísticos)
* Scikit-learn (para modelos de Machine Learning e pré-processamento)
