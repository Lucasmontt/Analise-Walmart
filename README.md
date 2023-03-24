# **Análise de Dataset da Rede Walmart**
Este relatório tem como objetivo apresentar a análise de dados de um dataset da rede Walmart, utilizando a linguagem de programação Python e suas bibliotecas.

## **A Questão do Négocio:**
A análise foi feita para a rede Walmart, que é uma multinacional estadunidense de lojas de departamento, e que deseja expandir uma de suas lojas, com o intuito de expandir sua presença em novos mercados e/ou melhorar sua posição competitiva no mercado o qual ela atua. O objetivo final desta análise é identificar qual a loja com maior potêncial de crescimento para realizar o investimento. Para isso, serão definidos alguns critérios e métricas para avaliação das lojas.

## **O Entendimento do negócio:**
A companhia foi fundada por Sam Walton em 1962, incorporada em 31 de 
outubro de 1969 e feita capital aberto na New York Stock Exchange, em 1972.  A rede vem em constante crescimento, onde no ano de 2021 teve um lucro de  $13.51 bilhões, sendo assim, uma das principais lojas de varejo do mundo. 

## **Estruturação do Dataset:**
O dataset da rede é composto, originalmente, por 8 variáveis que descrevem as vendas semanais das lojas e a situação econômica da região naquela semana, no período de 2010 à 2012, tais como: preço dos combustíveis, CPI (índice de Preço ao Consumidor, taxa de desemprego, entre outras, tendo um total de 6435 registros. Foram realizados tratamentos para visualização de outliers, e criação de novas variáveis no dataset para melhorar a análise.

#### **Coleta e Transformação dos Dados:**
A importação do dataset *`'Walmart.csv'`*, foi realizada e transformada em um dataframe, nomeado como *`df`*. 
Para transformar as datas (coluna *`'Date'`*) no tipo datetime, foi usada a seguinte linha de código: *`df['Date'] = pd.to_datetime(df['Date'], format = '%d-%m-%Y')`*
Com a função df.info(), obtemos algumas informações importantes, como por exemplo, os dtypes de cada variável, assim é possível conferir se todas as variáveis estão com os dtypes corretos.

![df info](https://user-images.githubusercontent.com/106493763/227619095-18c11d27-9b19-45d2-92ab-f742877bf2e7.png)

Para facilitar a análise das datas no dataset, foram criadas as colunas de mês e ano, sendo respectivamente, *`'Month'`* e *`'Year'`*.

![Month Year](https://user-images.githubusercontent.com/106493763/227619119-ea4b2f15-4347-49f6-a90e-9e3baed982c4.png)

Utilizando a função *`df.isna().sum()`*, é possível ver que não há dados nulos em nenhuma das colunas.

![null](https://user-images.githubusercontent.com/106493763/227619140-2a2bd244-2957-4d27-b3bd-25513af22b42.png)

Utilizando a função *`df.duplicated().sum()`*, é possível ver que não há linhas duplicadas no dataframe, pois o valor retornado é igual a 0.

## **Análise Exploratória dos Dados:**

Com o intuito de analisar vendas reais sem a interferência de sazonalidades, as quais podem aumentar os valores das vendas, os feriados serão retirados do dataframe a ser analisado. 
Então é criado o dataframe *`df_no_holidays`*, que não conta com semanas e meses de feriados.

#### **Verificando o preço médio semanal por loja:**

Com base no dataframe *`df_no_holidays`* é criado o dataframe *`df_weekly_sales`*, que é feito por meio de um agrupamento por meio da coluna *`'Store'`*, e calculando a média das outras variáveis, além de excluir as colunas *`'Holiday_Flag'`*, *`'Month'`* e *`'Year'`*.
A imagem abaixo mostra uma parte do *`df_weekly_sales`* ordenado pelo valor de vendas semanais, de maneira decrescente.

![df weekly sales](https://user-images.githubusercontent.com/106493763/227619165-46826540-1838-4f0f-bbb3-ee7c1c8941b3.png)

#### **Verificando qual loja possui maior valor de venda acumulada:**

Para isso, salvamos o valor da média da coluna *`'Weekly_Sales'`* do dataframe *`df_weekly_sales`*  na variável *`weekly_sales_mean`*, a qual tem o valor de 1019786.11.

Criando o dataframe *`df_sort_sales`*, o qual é feito com base no *`df_no_holidays`*, por meio de um agrupamento pela coluna *`'Store'`*, trazendo a soma dos valores de venda de cada loja, ordenado de maneira decrescente. Onde é possível ver que a loja com maior valor acumulado de vendas semanais é a loja 20, e a menor é a loja 33.

Utilizando o novo dataframe df_store_n20 , o qual possui apenas os valores de vendas da loja 20, que foi encontrada como a que tem maior valor de vendas acumuladas, e a variável *`weekly_sales_mean`*, podemos calcular quantas vezes essa loja ultrapassou a média do período, resultando em 119 vezes.

#### **Detecção e Tratamento de Outliers**

Utilizando o método de visualização de outliers, Boxplot, pode-se verificar os seguintes gráficos:

![boxplot](https://user-images.githubusercontent.com/106493763/227619217-51987bcd-35a6-4619-bebd-e83c01a1a8f8.png)

Onde, os valores acima não são considerados outliers, pois todos estão entre valores considerados normais, pois os valores temperatura podem ter uma grande disparidade dependendo da estação do ano, e a taxa de desemprego pode ser mais alta ou mais baixa dependendo da situação econômica da região no período.

## **Cálculo de Taxa Média de Crescimento Anual (2010 - 2012)**

Uma maneira de analisar as lojas e, assim chegar a decisão de qual loja realizar o investimento, é verificando a taxa média de crrescimento anual das mesmas. Para isso:

É preciso fazer o agrupamento das vendas por loja e ano. O que é mostrado abaixo pelo dataframe *`grouped_df`*:

![grouped df](https://user-images.githubusercontent.com/106493763/227619250-f31923f6-c6a6-4529-b0b8-5a123f3200ac.png)

Com os dados disponíveis, é possível calcular a variação percentual das vendas em relação ao ano anterior. Tal dado é encontrado na nova coluna *`'Percent_Var'`* do dataframe.

![percent var](https://user-images.githubusercontent.com/106493763/227619262-539c9754-fd1c-4644-86b4-1f4d63c109dc.png)

Após isso, é possível calcular a taxa de crescimento anual, mostrada no dataframe *`avg_growth`*. A imagem abaixo mostra apenas uma parte do dataframe mencionado.

![avg growth df](https://user-images.githubusercontent.com/106493763/227619279-999a5e34-22e2-42d5-9c43-30b43b7693e5.png)

Com o objetivo de investir em lojas que tenham um valor baixo em vendas, porém com uma boa taxa de crescimento, as lojas com maiores valores serão retiradas da análise, restando apenas as lojas que possuem vendas com valor abaixo de 50% das vendas da Loja 20, a qual é a loja com maior valor de vendas semanais, as quais são separadas no dataframe *`df3`*.

Como a taxa de crescimento é uma das métricas importantes para escolher uma loja, é aplicado um filtro, onde serão mantidas apenas as 5 lojas com maiores valores da taxa média de crescimento anual, dado que se encontra na coluna *`'Avg_Growth'`*. Estas lojas são:

![df 5 stores](https://user-images.githubusercontent.com/106493763/227619295-bd82afbc-3d24-44a2-a9e3-86d2b47795c9.png)

## **Visualização dos Dados:**

A seguir, são apresentados os gráficos que explicam as relações entre as variáveis:

#### **Vendas semanais máximas, médias e mínimas ao longo do tempo:**

Com base no gráfico abaixo, é possível concluir que mesmo excluindo os meses de feriados, ainda existe um aumento nas vendas das lojas em análise, devido à sazonalidades, as quais são referentes aos meses de novembro (Dia de ação de graças) e dezembro (Natal).

![grafico 1](https://user-images.githubusercontent.com/106493763/227619322-3a4b83ef-93e4-4ddc-8d9d-b8e48f9c1ae5.png)

#### Taxa média de crescimento anual de cada loja:

Apenas as lojas 7 e 38 passam da média das 5 lojas, e a loja 44 se aproxima muito da média (representado pela linha vermelha), sendo elas as maiores taxas de crescimento.

![grafico 2](https://user-images.githubusercontent.com/106493763/227619334-8a76f5a2-eea5-4f4b-9bad-78ecca2cd451.png)

#### Valor médio de vendas semanais de cada loja no período:

Apenas as lojas 7 e 17 passam da média de vendas das 5 lojas. Pode-se verificar também, que há um grande disparidade entre o a loja com maior valor e a loja com menor valor.

![grafico 3](https://user-images.githubusercontent.com/106493763/227619353-227734a2-179a-468f-a8c9-a02c69d65894.png)

#### Taxa média de desemprego nas regiões:

 Apenas as lojas 7 e 38 passam da média de vendas das 5 lojas, o que nesse caso não é um ponto positivo, mas ao ver que a loja 7 passa da média de vendas e da taxa de crescimento, pode-se concluir que a taxa de desemprego não é algo que afeta significativamente o desempenho da loja em suas vendas.

![grafico 4](https://user-images.githubusercontent.com/106493763/227619373-d0dd9a96-ae38-46f8-9d45-fc8f1dcd8b18.png)

#### CPI médio nas regiões:

Apenas as lojas 3 e 7 passam da média de vendas das 5 lojas, o que nesse caso não é um ponto positivo, mas ao ver que a loja 7 passa da média de vendas e da taxa de crescimento, pode-se concluir que o CPI não é algo que afeta significativamente o desempenho da loja em suas vendas.

![grafico 5](https://user-images.githubusercontent.com/106493763/227619566-d25e626f-4676-4517-9499-692028f5ca8d.png)

#### Visualização de correlação entre variáveis:

Todas as correlações com a variável *`'Weekly_Sales'`* possuem valores negativos, sendo assim, não afetam a vendas das lojas de maneira direta, assim como pode ser concluido analisando os gráficos acima.

![heatmap](https://user-images.githubusercontent.com/106493763/227619580-392c07ee-29a5-4105-905f-a5783486de19.png)

## Seleção da Loja à Ser Investida:

Para decidir qual a melhor loja para realizar o investimento, será calculado o crescimento das lojas nos próximos 5 anos, utilizando a fórmula de juros compostos:

 `FV = PV * (1 + r_mensal)^n`, onde:

- FV é o valor futuro das vendas após X anos;

- PV é o valor atual das vendas;
- r_mensal é a taxa média de crescimento anual, que deve ser convertida para a taxa de juros periódica (mensal, trimestral, semestral, etc.), que neste caso é convertida para uma taxa mensal.
	- Utilizando como exemplo, uma taxa de 10%, sendo 0.1:
	- r_mensal = (1 + r)^(1/12) - 1
	- r_mensal = (1 + 0.1)^(1/12) - 1
	- r_mensal = 0.007974
	
- n é o número de períodos (mensal, trimestral, semestral, etc.) em X anos. Como neste caso é usado taxas mensais, os períodos também devem ser calculados como mensais, sendo assim, 5 anos = 60 meses.

#### Após realizar o cálculo para cada loja, obtem-se no seguinte resultado:

![fv 5](https://user-images.githubusercontent.com/106493763/227619600-e53cd1b3-65be-4fcd-a7c3-8a657531cc04.png)

#### Ao fazer o cálculo para 10 anos, o resultado é:

![fv 10](https://user-images.githubusercontent.com/106493763/227619607-8603bc2f-5387-49bc-9226-c0af88dfc2a3.png)

## Loja Escolhida:

Após análise dos dados, investimento para expansão deve ser realizado na Loja 38, pois dentre as 5 lojas selecionadas como potêncial de investimento, ela possui a maior taxa média de crescimento anual e apresentou maior aumento nas vendas quando comparada às outras lojas, sendo 74,81% de aumento em 5 anos, e 205,58% de aumento em 10 anos.

Por mais que no período analisado ela tenha o segundo menor valor de vendas, seu valor acaba superando as outras lojas ao longo do tempo, por conta de sua alta taxa de crescimento anual.

#### Conclusão

Como o objetivo da rede é expandir a sua presença para novos mercados e/ou melhor sua posição competitiva no mercado em que atua, o foco nesta análise foi encontrar uma loja com um baixo valor de vendas, porém que tivesse um bom potêncial de crescimento.

#### Opção de investimento a ser realizado: 

O investimento pode ser direcionado para melhorar a experiência do cliente, aumentar a visibilidade da loja, reduzir os custos operacionais e ajustar a oferta de produtos ou serviços de acordo com as necessidades do mercado local.
