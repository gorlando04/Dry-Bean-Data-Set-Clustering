# Dry-Bean-Data-Set-Clustering


Este projeto tem como ideia realizar um agrupamento em um conjunto de dados relacionado a espécies de feijáo que foi encontrado no Kaggle. O conjunto de dados pode ser encontrado no seguinte link:

Link: https://www.kaggle.com/datasets/muratkokludataset/dry-bean-dataset

# Aprendizado Não-Supervisionado e Agrupamento

# Conjunto de dados

O Dataset escolhido é de feijões secos
que podem ser separados em 7 tipos
distintos. É composto de 16 atributos + classe, sendo 14 atributos numéricos contínuos e 2 atributos numéricos discretos, felizmente o  dataset não possui nenhum valor
nulo em nenhuma classe

## Atributos

COmo se trata de uma tarefa de aprendizado não supervisionado é interessante entender como alguns atributos estão dispostos no conjunto de dados.

### Area

Atributo Númerico Discreto.
A área de uma zona de feijão e o número de pixels dentro de seus limites.
Este atributo não possui nenhum valor nulo.

Podemos ver como este atributo está disposto na imagem abaixo

![image](https://user-images.githubusercontent.com/91696970/236012423-34a70ac4-6171-4d27-9272-e07bb365ed9b.png)

### Compactidade

Este atributo é um numérico contínuo.
Este valor representa o quão compacto o grão de feijão coletado para amostra é.
Este atributo não possui nenhum valor nulo

Podemos ver como este atributo está disposta na imagem abaixo:

![image](https://user-images.githubusercontent.com/91696970/236012620-2734f935-2919-4a90-a411-0cce7be30bb0.png)


### Classe

O atributo classe possuia 7 valores, mas como se trata de agrupamento o atributo classe não é relevante, visto que não se está tentando predizes os valores, mas sim agrupá-los em grupos por similaridade. 
Desta maneira, o atributo classe será excluído.

# Pré-processamento

O agrupamento é uma tarefa que é muito sensível a como os dados estão dispostos no espaço do conjunto de dados, assim é necessário tratar corretamente os dados para evitar que haja algum tipo de problema na hora da execução do 
algoritmo.

Logo, foram realizados dois processos. Normalização e PCA

## Normalização

Este um dos processos muito utilizados no aprendizado de máquina visto que é interessante que os dados estejam padronizados em um intervalo para que nenhum atributo seja dominantes em relação aos outros. A normalização realizada foi a padrão, utilizando média e desvio padrõa.

## PCA

Para enxergar em 2 dimensões o conjunto escolhido, realizamos um método
de redução de dimensionalidade para plotar o gráfico, o PCA. O PCA é baseado na variância dos dados, ou seja, ele tenta criar uma nova representação dos
dados, com uma dimensão menor, mantendo a variância entre eles.

Com o PCA foi possível verificar como os dados ficariam caso utilizássemos duas dimensões

![image](https://user-images.githubusercontent.com/91696970/236014329-507b0784-edd7-487b-aab8-578ab6d3a3ca.png)

Entretanto esta ideia foi descartada, visto que as variâncias resultantes não permitiam identificar quais atributos deveriam ser descartados. Mas é possível perceber que existe uma clara diferença entre dois principais grupos, sendo um grupo possuindo muitos pontos sobrepostos, que pode dificultar a performance do algoritmo.

# Como será realizado o agrupamento

Neste projeto, serão testados 3 tipos de algoritmos diferentes de agrupamento.- Para cada algoritmo, diferentes configurações e parâmetros serão utilizados para que haja uma ampla testagem dos métodos de agrupamento
Entretanto, como se está lidando com um conjunto de dados que contém 16 atributos, é inviável construir um grafo que permita a visualização geral dos dados, já que seria necessário um gráfico com 16 dimensões, o que não é trivial

# Agrupamento

## K-MEANS

Nessa etapa, o algoritmo do K-Means será testado.
Ele será utilizado com diversos valores de K possíveis e também terá como algoritmo de inicialização o k-means++

Para verificar qual foi o melhor valor de k, foi utilizado dois testes, o da Silhueta e o teste do cotovelo, ambos podem ser visualziados abaixo.

![image](https://user-images.githubusercontent.com/91696970/236015288-66908897-0505-493f-932a-7d495816b91a.png)

![image](https://user-images.githubusercontent.com/91696970/236015308-4ab84f20-1179-4f33-acd4-05b4e23e9ccd.png)

É possível compreender com isto, que um bom valor para k seria 5, tendo em vista que é possível observar a forma de um cotovelo quando se observa o erro quadrático medio pelo valor de k. Lembrando-se que incialmente os dados
estavam divididos em 7 classes.

Com isso, a distribuição dos grupos formados foi a seguinte:

![image](https://user-images.githubusercontent.com/91696970/236015551-432ce7a9-bdcb-4269-af2d-cb695f470b20.png)

## HDBSCAN

HDBSCAN, Métrica = euclidiana

Foram testados com 5 valores para o número de pontos mínimos do cluster e, a cada
iteração, foi testado o índice de silhueta. A partir deste, foi escolhido o melhor

Diferentemente do k-MEANS o HDBSCAN não possui um hiperparâmetro tão impactante, embora possui o tamanho mínimo do cluster, que define se a região que o ponto está é de alta densidade ou não, é interssante notar, que o HDBSCAN também verifica pontos que são outliers.

Para este algoritmo, pode-se verificar o valor da silhueta abaixo:

![image](https://user-images.githubusercontent.com/91696970/236016005-f059875b-ecc6-40ad-8919-01ed23721128.png)


O valor escolhido para o tamanho mínimo do cluster foi de 30, e a aŕvore condensada pode ser verificada abaixo

![image](https://user-images.githubusercontent.com/91696970/236016185-3551aedc-b255-4f88-a667-5b7c1711871a.png)

Nota-se que o HDBSCAN encontrou 2 grupos claros, que é o que foi compreendido pelo PCA, e o restante não obteve densidade suficiente para formar um grupo.

# Comparação resultados

Embora os algoritmos aglomerativos não foram exibidos, eles foram testados neste projeto. Abaixo, pode-se visualizar uma comparação entre os algoritmos.

![image](https://user-images.githubusercontent.com/91696970/236016572-fee71076-9adc-44f9-a12f-9c112facf493.png)


# Conclusão

Ao analisar a métrica de validação utilizada (índice de silhueta), o melhor método de agrupamento foi o Algoritmo Aglomerativo. Sendo que as medidas de ligação média e ligação simples empataram em 0.7361 com um total de 2 clusters.

Apesar do número de clusters escolhidos com o SSE do k-means ser 5, ele não teve um bom desempenho com a métrica da silhueta, pois o SSE só leva em consideração a distância intra-grupos do clusters formados, enquanto a silhueta leva em consideração a distância intra-grupos e a distância inter-grupos.

Como o HDBSCAN é um algoritmo que, além de baseado em densidade, é hierárquico, ele é muito poderoso. Como visto no gráfico construído pelo método do PCA existia uma clara separação em 2 grupos. Um grupo pequeno e um grupo muito grande, com diversos pontos. Isso é representado na árvore condensada gerada pelo modelo do HDBSCAN, em que há uma partição com poucos pontos e outra com vários pontos. Entretanto, no grupo com muitos pontos os sub-grupos que podem ser gerados não tiveram a densidade suficiente para conseguir gerar um novo grupo, e dessa forma não houve uma divisão do grupo grande, oque reflete no resultado do HDBSCAN, que mostra a presença de 2 grupos

O Algoritmo Aglomerativo com ligação simples é um método muito útil para encontrar grupos baseados em densidade, ou seja, quando o conjunto de dados tem uma distribuição muito densa dos pontos, como o conjunto que está sendo estudado, ele consegue identificar melhor os grupos. Assim, o resultado do Algoritmo Aglomerativo com ligação simples é explicável, já que como este método é poderoso para identificar grupos densos, quando o número de clusters escolhido é 2 há uma maior densidade dos grupos, visto que há uma clara separação dos pontos do conjunto de dados analisado.

O Algoritmo Aglomerativo com ligação completa é um método que não é tão sensível a outliers mas que tende a quebrar grupos muito grandes, que é o caso do conjunto de dados estudado. Dessa forma, o resultado do Algoritmo Aglomerativo com ligação completa não obteve resultados tão bons quanto ao algoritmo configurado com outros parâmetros, pois este método quebra os grupos grandes em menores, o que explica o por que quando o número de clusters escolhido foi 2, o algoritmo não obteve o melhor desempenho..

Algoritmo Aglomerativo com ligação média é um misto entre a ligação completa e a simples, sendo menos sucetível a ruídos e com mais tendência a identificar grupos mais densos. Assim, o resultado do Algoritmo Aglomerativo com ligação média é muito semelhante ao método da ligação simples, visto que a ligação média tem características da ligação simples, como por exemplo, a identificação de grupos densos, que é um atributo importante no conjunto de dados trabalhado.

Por fim, como nosso dataset se trata de diversas características de feijões é possível analisar que nossos dados podem ser divididos em 2 grupos de feijões que realmente se diferem em suas características, que foi o resultado que adotamos, pois mais divisões nesse grupo de acordo com nossos algoritmos, seriam possíveis, mas já acrescentaria sobreposições nos grupos e os feijões não teriam mais tantas diferenças e começariam a ser mais similares entre os grupos.

# Participantes

Augusto Luchesi Matos




Carlos Eduardo Nascimento dos Santos



Gabriel Meirelles Carvalho Orlando
