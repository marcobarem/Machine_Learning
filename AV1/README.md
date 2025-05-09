# Clusterização de Clientes de Telecomunicações

## Descrição do Projeto

Este projeto tem como objetivo realizar uma análise de clusterização em um dataset de clientes de uma empresa de telecomunicações. O principal objetivo é segmentar os clientes em grupos distintos com base em seu perfil de consumo, características demográficas e serviços contratados. A identificação desses segmentos pode auxiliar a empresa na tomada de decisões estratégicas, como personalização de ofertas, otimização de campanhas de marketing, melhoria do atendimento e desenvolvimento de estratégias de retenção mais eficazes.

## Dataset

O dataset utilizado neste projeto é o "Telecom Customer Data", publicamente disponível no seguinte link:
[https://raw.githubusercontent.com/klaytoncastro/idp-machinelearning/refs/heads/main/datasets/telecom-customer-data.csv](https://raw.githubusercontent.com/klaytoncastro/idp-machinelearning/refs/heads/main/datasets/telecom-customer-data.csv)

Ele contém informações sobre:
* Dados demográficos dos clientes (gênero, se é idoso, parceiro, dependentes).
* Serviços assinados (telefone, múltiplas linhas, serviço de internet, segurança online, backup online, proteção de dispositivo, suporte técnico, streaming de TV e filmes).
* Informações da conta (tempo de contrato/tenure, tipo de contrato, faturamento sem papel, forma de pagamento, gastos mensais e totais).
* Informação sobre Churn (se o cliente cancelou ou não o serviço).

## Tecnologias e Bibliotecas Utilizadas

* **Python 3.x**
* **Pandas:** Para manipulação e análise de dados.
* **NumPy:** Para operações numéricas.
* **Matplotlib e Seaborn:** Para visualização de dados.
* **Scikit-learn:** Para as etapas de pré-processamento, modelagem de clusterização (K-Means), avaliação de métricas (Silhouette Score) e otimização de hiperparâmetros (GridSearchCV).

## Metodologia e Estrutura do Notebook

O notebook `Telecom.ipynb` está estruturado da seguinte forma:

1.  **Bloco 1: Configuração Inicial e Carregamento dos Dados**
    * Importação das bibliotecas necessárias.
    * Carregamento do dataset a partir da URL fornecida.

2.  **Bloco 2: Inspeção Rápida e Limpeza de Dados**
    * Visualização inicial dos dados (`head()`, `info()`).
    * Tratamento da coluna `TotalCharges` (conversão para tipo numérico e preenchimento de valores ausentes com a mediana).
    * Remoção da coluna `customerID` (irrelevante para a clusterização).
    * Verificação final de valores ausentes.

3.  **Bloco 3: Pré-processamento dos Dados**
    * Identificação de features numéricas e categóricas.
    * Aplicação de `StandardScaler` nas features numéricas para padronização.
    * Aplicação de `OneHotEncoder` (com `drop='first'`) nas features categóricas para convertê-las em formato numérico, evitando multicolinearidade.

4.  **Bloco 4: Determinando o Número Ideal de Clusters (k)**
    * Utilização do **Método do Cotovelo (Elbow Method)**, analisando o WCSS (Within-Cluster Sum of Squares) para diferentes valores de `k`.
    * Cálculo do **Coeficiente de Silhueta** para diferentes valores de `k` para avaliar a qualidade da separação dos clusters.

5.  **Bloco 5: Aplicando o K-Means e Adicionando Clusters ao DataFrame Original**
    * Escolha do número ótimo de clusters (`k_otimo`) com base nos resultados do Método do Cotovelo e do Coeficiente de Silhueta (resultando em k=4).
    * Instanciação e treinamento do modelo K-Means.
    * Adição dos rótulos dos clusters ao dataframe original para análise.

6.  **Bloco 6: Análise e Interpretação dos Clusters**
    * Cálculo das médias das features numéricas para cada cluster.
    * Conversão da feature `Churn` para formato numérico para análise da taxa de churn por cluster.
    * Visualização da distribuição de features categóricas chave (como `Contract`, `PaymentMethod`, `InternetService`, `Churn`) por cluster usando `countplot`.
    * Visualização dos clusters em um espaço 2D usando Análise de Componentes Principais (PCA) e plotagem dos centroides.
    * Instruções para interpretação dos perfis dos clusters.

7.  **Bloco 7: (Bônus) Otimização de Parâmetros do K-Means com GridSearchCV**
    * Demonstração de como o `GridSearchCV` pode ser usado para testar diferentes combinações de hiperparâmetros do K-Means (como `n_clusters`, `init`, `n_init`).
    * Utilização de uma função de pontuação personalizada baseada no `silhouette_score` para avaliação no `GridSearchCV`.
    * Apresentação dos melhores hiperparâmetros encontrados e o respectivo score.

## Resultados Principais

A análise indicou que **k=4 clusters** é uma boa escolha para segmentar os clientes, confirmada tanto pela análise manual do Coeficiente de Silhueta (score de 0.2689) quanto pelo GridSearchCV (score de 0.2684 com `n_clusters=4`).

As personas preliminares identificadas para cada cluster (com k=4) são:

* **Cluster 0: "Clientes de Longa Duração e Alto Valor"**
    * Características: `tenure` alto (média ~59 meses), `MonthlyCharges` e `TotalCharges` elevados.
    * Comportamento: Baixa taxa de `Churn` (média ~12%). Geralmente associados a contratos mais longos (ex: Two year).
* **Cluster 1: "Clientes em Risco/Novos com Serviços Moderados"**
    * Características: `tenure` baixo (média ~16 meses), `MonthlyCharges` e `TotalCharges` moderados/baixos.
    * Comportamento: Segunda maior taxa de `Churn` (média ~40%). Frequentemente com contratos mensais e podem utilizar serviços como Fiber optic.
* **Cluster 2: "Clientes Econômicos/Básicos Leais"**
    * Características: `tenure` moderado (média ~31 meses), mas com `MonthlyCharges` e `TotalCharges` significativamente baixos.
    * Comportamento: Menor taxa de `Churn` (média ~7%). Tendem a ter contratos de um ou dois anos e podem não ter serviço de internet ou usar DSL básico.
* **Cluster 3: "Seniors em Risco com Serviços Premium"**
    * Características: Todos são `SeniorCitizen`, `tenure` baixo/moderado (média ~29 meses), `MonthlyCharges` e `TotalCharges` elevados.
    * Comportamento: Maior taxa de `Churn` (média ~47%). Frequentemente associados a contratos mensais e serviços como Fiber optic.

## Como Executar o Notebook

1.  Clone o repositório (se aplicável) ou baixe o arquivo `Telecom.ipynb`.
2.  Abra o notebook em um ambiente Jupyter (como Jupyter Lab, Jupyter Notebook clássico, ou Google Colab).
3.  Certifique-se de que todas as bibliotecas listadas na seção "Tecnologias e Bibliotecas Utilizadas" estão instaladas. No Google Colab, a maioria já vem pré-instalada.
4.  Execute as células do notebook em ordem sequencial.
5.  **Importante:** No Bloco 5, após a análise dos gráficos do Método do Cotovelo e do Coeficiente de Silhueta (Bloco 4), confirme ou ajuste a variável `k_otimo` com o número de clusters que julgar mais apropriado para a sua análise.