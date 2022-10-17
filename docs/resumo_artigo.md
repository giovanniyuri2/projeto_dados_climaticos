# Informações Iniciais
- O objetivo é combinar Redes Complexas com Machine Learning para analisar as fases do El Niño Southern Oscillation (ENSO). Este fenômeno não linear consiste de (des)aumento anômalo de temperatura no Oceano Pacífico tropical, que tem ocorrência irregular e causa variabilidade climática em todo o mundo.
- Construímos Redes Climáticas temporais a partir da série temporal de Temperatura do Ar na Superfície e calculamos métricas de rede para caracterizar os episódios quentes e frios do ENSO.
- A rede complexa representa a relação entre as diferentes regiões do planeta e o aprendizado de máquina cria modelos para classificar as diferentes classes de ENSO.
- A principal característica é uma variabilidade fora da temperatura esperada da superfície do mar (TSM) nesta região. A fase positiva acontece quando a TSM média é mais quente do que o esperado na região ENSO, que é chamada de El Niño. A fase oposta, mais fria que a SST média, é conhecida como La Niña. Essa flutuação do ciclo é responsável por mudanças na precipitação e temperatura ao redor do mundo, conhecidas como teleconexões.
- ONI: Oceanic Niño Index

# Problema de Pesquisa
- Em vez de realizar uma tarefa de previsão ou agrupamento para prever algum descritor ENSO, aqui, pretendemos classificar e entender o padrão de intensidade do episódio El Niño ou La Niña, conforme descrito nos rótulos internos da Fig. 1. (ver no artigo).
- Neste problema de classificação, o objetivo é reconhecer os padrões climáticos anteriores em todo o mundo que podem desencadear uma condição ENSO forte/fraca.
- Por exemplo, a Fig. 1 mostra o episódio completo do El Niño entre 1986 e 1988. Podemos considerar as anomalias de temperatura dos 365 dias anteriores, construir o CN (Climate Networks) correspondente e reconhecer os padrões topológicos associados à ativação e intensidade dos eventos ENSO por alguns Algoritmos de ML.
- A proposta e novidade deste trabalho é empregar classificação para análise ENSO usando características topológicas de alto nível das redes temporais.
- Mais recursos topológicos permitem que mais possibilidades sejam exploradas do que usar apenas os valores das séries temporais para realizar as previsões.
- Além disso, alguns algoritmos de classificação não dependem de big data para obter bons resultados, enquanto as abordagens de aprendizado profundo precisam de uma grande quantidade de instâncias para obter treinamento adequado e bons resultados, o que é uma limitação no caso de dados climáticos históricos.

# Materiais e Métodos
## Dados
- Usamos os dados globais diários de temperatura do ar (SAT) perto da superfície (1000 hPa) da National Oceanic and Atmospheric Administration (NOAA) e o projeto Reanalysis I do National Center for Environmental Prediction/National Center for Atmospheric Research (NCEP/ NCAR).
- Optamos por utilizar este conjunto de dados ao invés do SST devido à inclusão das áreas terrestres, que é uma informação extra que consideramos importante para as análises. Além disso, a inclusão de dados globais é relevante uma vez que a Oscilação do El Niño não é um fenômeno isolado no pacífico central, mas conectado com muitas outras regiões climáticas do mundo como um sistema complexo.
- O conjunto de dados abrange o período de 1948 a 2018, com a informação da temperatura diária de pontos espaçados de 2,5 a 2,5 graus de latitude e longitude resultando em um total de 10512 pontos ao redor do globo, ou seja, áreas terrestres e marítimas. Retiramos os pontos localizados nos pólos, considerando apenas os dados geográficos localizados entre as latitudes 66:5 N e 66:5 S. Essa estratégia ocorre porque a alta densidade de pontos localizados nos polos influenciou negativamente a análise.
- Depois disso, calculamos a temperatura média de longo prazo centrada em cada um dos 365 dias do calendário, para remover a sazonalidade da temperatura diária em cada ponto espacial.
- Em outras palavras, a climatologia de 365 dias é a média acima de 70 amostras do mesmo dia entre todos os anos.
- Para cada dia do calendário, subtraímos sua temperatura média de longo prazo correspondente, obtendo as séries temporais de anomalias SAT dos pontos da grade. Retiramos os dados correspondentes a 29 de fevereiro no caso de anos bissextos. Como resultado, as anomalias SAT (SATa) são os dados brutos usados aqui para construir os CNs.

## ANÁLISE TEMPORAL CN E ML
- Inicialmente, dividimos o conjunto de dados espaço-temporais SATa em janelas sobrepostas de $\Delta{t}$ dias para todos os pontos espaciais.
- Dado que o ciclo sazonal climático completo do planeta é de um ano (quatro estações) e a natureza interanual do ENOS, fixamos
$$\Delta{t}=\text{365 dias}$$
- A janela deslizante é movida a cada mês, em que a referência é o último mês do intervalo, por exemplo:

(1 de julho de 1950 - $\Delta{t}$, 1 de julho de 1950), (1 de agosto de 1950 - $\Delta{t}$, 1 de agosto de 1950),..., (Mês-ano - $\Delta{t}$, Mês-ano)
- Por exemplo, ao falar do CN de dezembro de 2019 (m.yy) estamos nos referindo à série temporal de SATa no intervalo (12.2018, 12.2019) u seja, o CN do ano anterior, e inferindo a classe do episódio ENSO para os próximos três meses de acordo com as propriedades topológicas do CN em (m.yy).
- O CN temporal (G) é o conjunto de CNs de todos os meses no conjunto de dados, isto é, (ver no artigo).
- com l o número de camadas ou janelas deslizantes e o sobrescrito (m.yy) é o mês de referência. Este CN temporal contém não apenas a semelhança entre os nós, mas também a evolução global por meses do sistema de temperatura climática.
- Para cada janela deslizante ($G_{x}^{m.yy}$) calculamos a correlação de Pearson entre todas as séries temporais, produzindo a matriz de similaridade entre os pontos espaciais ou nós.
- A partir da matriz de similaridade ou correlação, estabelecemos conexões entre nós quando os valores de correlação absoluta são superiores a 0:65, com base em trabalhos relatados anteriormente.
- Portanto, esta é a matriz de adjacência que representa o CN do SATa do ano anterior até o mês de referência (m.yy). Cada rede $G_{x}^{m.yy}$ contém a dinâmica climática e similaridade dos nós ao redor do globo, cujas feições topológicas podem descrever a formação e evolução do fenômeno ENSO.
- Aqui, buscamos entender a evolução do El Niño e La Niña a cada mês com as informações do CN construído com o SATa do ano anterior.
- Para conduzir os experimentos de classificação, calculamos várias medidas estruturais para cada um dos CNs em G e colocamos as novas informações em um novo conjunto de dados de atributos.
- As medidas adotadas são a transitividade ($\tau$), o número de enlaces (M), o grau médio (<$k$>), comprimento médio do caminho mais curto (<$l$>), a modularidade (Q), a distância média global do link (<<$d$>>), e as médias de Coreness Centralidades (Co), Excentricidade (Ec) e Autovetor (Ev).
- Além disso, cada um dos 822 $G_{x}^{m.yy}$ CNs tem seu valor ONI, que corresponde aos próximos três meses da referência, isto é, [(m+1.yy), (m+2.yy), (m+3.yy)].
- Assim, consideramos as três configurações experimentais a seguir:
  - Dataset1, duas classes com El Niño (EN) e nenhuma ocorrência de El Niño ou qualquer outra coisa (AE);
  - Dataset2, três classes com ocorrências de El Niño (EN), La Niña (LN) e anos regulares (RY), o que significa que nem El Niño nem La Niña ocorreram;
  - Dataset3, cinco classes com forte El Niño (SEN), fraco El Niño (WEN), forte La Niña (SLN), fraco La Niña (WLN) e ano regular (RY).
- Assim, exploramos um problema de classificação binária e dois multiclasses.

## Avaliação Estatística
- Dividiu os datasets em treino e teste e fez k-fold cross validation, com k=10.
- Calculou diversas métricas (acurácia, recall, precisão, AUC, etc).



