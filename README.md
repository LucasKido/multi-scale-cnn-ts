# CNN Multi-Escala para Série Temporal

## Introdução
Aplicação de uma Rede Neural Convolucional de Multi-Escala para Série Temporal, baseada no paper [Multi-Scale Convolutional Neural Networks for Time Series
Classification](https://arxiv.org/pdf/1603.06995.pdf), mas com algumas alterações pois o paper é uma classificação e na aplicação será uma regressão.

## Objetivo
Construir o modelo, compreender e enteder como funciona o modelo para futuras aplicações.

## Modelo
O modelo é uma rede convolucional onde utiliza a convolução 1D nas séries temporais na entrada para extrair as variáveis recebidas.
Como o artigo tem o intuito de criar um modelo *end-to-end*, onde tenta resolver um problema de conseguir captar variáveis de diferentes escalas de tempo, para isso a arquitetura do modelo é dividida em 3 estágios sequenciais: a transformação, convolução local e convolução global, que tenta resolver o objetivo proposto no paper.

### Etapa de Transformação
Aqui é aplicada diversas transformações na série temporal afim de criar diversas entradas para extrair variáveis em diferentes escalas de tempo.
* **Original**: série temporal sem nenhuma alteração, mas como há alteração de classificação para regressão, vamos utilizar um pedaço da série temporal, no caso 3 dias;
* **Multi-escala**: um pedaço da série temporal em diversas escalas, afim de obter tendências de diferentes escalas, no caso *down-sampling*;
* **Multifrequência**: aplicado médias móveis com o intuito de remover perturbações de alta-frequência e ruídos aleatórios.

### Etapa de Convolução Local
Após a transformação das entradas é feita uma convolução 1D em cada entrada, usando mesmo tamanho de filtro para todas as séries temporais, desta forma algumas saídas das convoluções terão diferentes tamanhos. Para resolver esse problema cada saída é passada por um *Max Pooling* para deixar todos os tamanhos iguais, utilizando o *pooling factor p*.

### Etapa de Convolução Global
Nesta etapa é utilizado *deep concatenation* para concatenar verticalmente todas as variáveis mapeadas, para depois ser passada em uma camada de convolução 1D e *max pooling* e depois "achatada" para uma camada densa e a camada de saída.

![Arquitetura da Rede](https://github.com/LucasKido/multi-scale-cnn-ts/blob/main/image/cnn_arch.PNG "Arquitetura da Rede")

## Conclusão
Podemos ver que o modelo possui uma performance razoável, porém seria necessário testar mais opções para verificar o modelo mais a fundo, porém necessitaria de mais recursos computacionais para realizar isso.
* Aumentar a quantidade de dias no pedaço da série temporal original;
* Ao invés de down-sampling usar mais dias que a entrada original para captar tendências mais longas;
* Utilizar diferentes médias móvel, para verificar as médias móveis ótimas.