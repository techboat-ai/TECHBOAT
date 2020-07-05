# TECHBOAT
Repositório com o código fonte do projeto TECHBOAT

**O protótipo desenvolvido é apresentado em duas partes:**

1.   Modelo de Machine Learning & Real-Time Data (Simulação)
2.   Data Analysis

## **[Parte 1]** 

## **Modelo de Machine Learning**

*Nesta etapa, o modelo de Machine Learning de classificação é construído.*

*O modelo tem como objetivo **prever se ocorrerá um acidente ou não**, com base em variáveis que descrevem a operação de manobra -- ou seja, a configuração do sistema {navio rebocador, ambiente, navio rebocado}.*

*O modelo construído é uma Regressão Logística, devido a seu alto grau de interpretabilidade: o modelo prevê **qual é a probabilidade de acidente**, com base nas variáveis de configuração do sistema de interesse.*

*A seguir, as principais etapas para a construção do modelo são discutidas.*

**Criação de Base de Dados**

Nesta etapa, a base de dados utilizada para o treinamento do modelo de Machine Learning é gerada.

A base contém as seguinte features (variáveis preditoras):

- Variáveis relativas ao rebocador:

  - "velocidade_reb" : velocidade do rebocador em nós/s;
  - "potencia_reb" : potência do rebocador em W;
  - "massa_reb": massa do rebocador em ton;

- Variáveis relativas ao ambiente:

  - "velocidade_vento" : velocidade do vento em nós/s;
  - "angulo_deslocamento" : ângulo entre o vetor de onda das ondas do mar e do deslocamento do rebocador;
  - "visibilidade" : visibilidade em metros;
  - "distancia_entre_obstaculo" : distância entre o rebocador e um obstáculo que não seja o navio rebocado;

- Variáveis relativas ao navio rebocado:
  - "distancia_reb" : distância entre rebocador e navio rebocado;
  - "tonelagem_porte_bruto" : massa total (tonelagem de porte bruto) do navio rebocado;
  - "velocidade_relativa" : velocidade relativa entre rebocador e navio rebocado.

Os intervalos de valores assumidos por cada uma das features fora determinado a partir de dados de literatura especializada.

A base contém o seguinte target (variável predita):

- "acidente": indica se houve acidente (valor: 1) ou não (valor: 0).

Uma vez que os dados não nos foram disponibilizados, a base de dados para a criação do modelo foi elaborada pela equipe. A base foi construída de modo que as principais variáveis correlacionadas à ocorrência de acidente fossem calibradas para tal. Assim, é importante notar que a base de dados utilizada consiste de _mock data_, embora as correlações esperadas em um cenário real tenham sido mantidas. Assim, tanto a metodologia quanto as análises deste protótipo são facilmente estendíveis a cenários de dados reais.

**Treinamento do Modelo**

Neste passo, o modelo de Machine Learning é treinado.

Antes do treinamento, os dados são separados em dados de treino e de validação, com estratificação uniforme do target e 2/3 do volume de dados tomado para treino.

O modelo é construído a partir da respectiva classe do Sklearn. Os parâmetros default de regularização são mantidos.

Após o treinamento, a performance do modelo é avaliada utilizando os dados de validação e o classification report.


## **Simulação**

*Nesta seção é realizada a mensuração de risco de acidente devido a colisão durante a operação dos rebocadores.* 

*Esta etapa da solução consiste em obter as informações do rebocador e do ambiente, utilizando o modelo desenvolvido para informar em tempo real ao comandante sobre a probabilidade de ocorrência de uma colisão que resulte em acidente.*

**Base de Simulação**

Nesta etapa, a base de dados para a simulação é criada. As diretrizes para sua criação são as mesmas da base de treino.

**Probabildidade de Acidente ao Longo do Tempo - gráficos dinâmicos**

Nesta etapa, são gerados gráficos dinâmicos que ilustram a variação das variáveis de interesse, e como estas afetam a probabilidade de ocorrência de acidente.

Os gráficos são atualizados dinamicamente no tempo, simulando uma situação real de mensuração das variáveis de interesse em tempo real.

**Probabildidade de Acidente ao Longo do Tempo - valores numéricos**

Em uma situação real, espera-se que o resultado do modelo seja informado ao comandante do rebocado em tempo real, durante a operação. Sendo assim, pode ser mais interessante que os valores nos gráficos acima sejam apresentados de forma numérica como medidas das variáveis de interesse e previsão da probabilidade de acidente.

## **[Parte 2]**
## **Data Analysis**

*Esta etapa da solução compreende a análise do histórico de colisões. A partir dela são detectadas as variáveis mais correlacionadas com colisões que resultam em acidentes, tornando possível o desenvolvimento de treinamentos direcionados às principais causas.*  

**Matriz Correlação**

Nesta etapa, é calculada e visualizada a matriz de correlação entre todas as colunas da base de treino.

De particular interesse é a correlação das features com o target (compreendida nas última linha ou coluna da matriz). Com  a visualização, fica evidente quais são as variáveis mais correlacionadas (positiva ou negativamente) com a ocorrência de acidentes.

**Visualização da correlação - gráfico de barras**

A seguir, a correlação entre as features e o target é apresentada na forma de um gráfico de barras. Com isso, é facilmente verificável quais são as variáveis mais correlacionadas com a ocorrência de acidentes, de modo a direcionar a atenção e esforços de treinamento.**Distribuição: Acidente vs Não Acidente**

Na visualização a seguir, são exibidas as distribuições de cada uma das features, segregadas de acordo com o valor do target.

Na visualização, é imediata a identificação das varia´veis que esão mais relacionadas com a ocorrência de acidentes, e quais são os intervalos em que tal propensão ocorre:

- Para pares de preditores que apresentam uma concentração de pontos com valor 1 (ocorrência de acidentes), há evidência de que o par é relevante para a ocorrência de acidentes. Isto também se observa pela distribuição mais concentrada, o que indica intervalos particulares das respectivas features que levam a acidentes. Por esta razão, estas são as variáveis que merecem mais atenção e esforços de controle, alcançado através dos treinamentos;

- Para preditores sem uma concentração de pontos com valor 1 (ocorrência de acidentes), a relação acima não é observada, e, portanto, estes preditores não demandam especial atenção.

**Distribuição: Variáveis mais relevantes**

Nesta etapa, são selecionadas as variáveis mais relevantes (de forma automática, através dos coeficientes de correlação), e sua distribuição é exibida. O objetivo desta visualização é promover maior detalhe sobre os intervalos de valores das variáveis relevantes relacionados à maior probabilidade de acidente.

**Estatística Descritiva: Variáveis mais relevantes**

Nesta etapa, o valor médio das variáveis de interesse é calculado, tanto nos casos em que há acidente, quanto nos casos sem acidente. Essa informação é de grande relevância para o correto mapeamento das condições associadas à ocorrência de acidentes, o que auxilia na determinação dos treinamentos adequados.

