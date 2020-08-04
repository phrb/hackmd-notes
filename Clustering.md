# Clustering

- ~400 trajetórias de lobos
- Queremos encontrar os clusters do DBSCAN pra cada uma
    - Isso pode ser um baseline de comparação

## Qualidade de Fit

Encontrei essas três métricas de "qualidade de fit" para algoritmos de clustering, já implementadas no próprio scikitlearn:

1. https://scikit-learn.org/stable/modules/clustering.html#silhouette-coefficient
2. https://scikit-learn.org/stable/modules/clustering.html#calinski-harabasz-index
3. https://scikit-learn.org/stable/modules/clustering.html#davies-bouldin-index

Essas métricas são independentes da ground truth, e podem ser interessantes.
No fim, podemos compor nossa métrica de desempenho para um algoritmo de clustering no nosso dataset de animais usando essas métricas independentes da verdade, os clusters encontrados pelo DBSCAN, e o tempo de execução.

O composição mais ingênua seria uma média ponderada entre os 3 critérios acima, o tempo de execução, e a distância dos clusters preditos para os clusters do DBSCAN.
A média do critério acima calculado para cada trajetória no seu dataset alvo deve ser uma maneira justa de avaliar o desempenho de um algoritmo.

$$
\mathcal{P}(x_1, \dots, x_n) = \dfrac{(w_1m_1 + w_2m_2 + w_3m_3 + w_4d_{DBSCAN})}{w_1+w_2+w_3+w_4}
$$

## Plano

1. Decidir o dataset de animais alvo do nosso estudo
2. Gerar a "ground truth" com o DBSCAN (ou com hierarchical clustering? https://scikit-learn.org/stable/modules/clustering.html#hierarchical-clustering)
3. Implementar a métrica acima, ou alguma variação dela, no Scikitlearn
4. Avaliar os 5 algoritmos que você implementou usando o dataset, a ground truth calculada, e a métrica
    - Usar os espaços de busca que você definiu, e tentar modelar o desempenho com desenho de experimentos

## DBSCAN

- Clusterização usando tempo como variável:
    - Pode ser interessante misturar tempo com variáveis de espaço
    - Comparar clusters com espaço e tempo+espaço


## Calculando a Ground Truth

1. Implementar a heurística para seleção do $\varepsilon$ do artigo do Martin Ester
    - Pra cada trajetória do dataset de lobos:
        - Calcular todas as distâncias entre pontos
        - Encontrar a distância até o $k$-ésimo vizinho mais próximo, pra cada ponto
        - Ideia: Escolher a proporção de ruído ($d_{noise}$ = 0.1)
        - Descartar as $100d_{noise}$% maiores distâncias
        - $\varepsilon$ é último ponto da lista de distâncias
2. Usar a heurística do Martin para calcular os centroides de cada trajetória
3. Salvar essas trajetórias $(id_{trajetória}, centroides)$