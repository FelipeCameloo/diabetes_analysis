https://www.kaggle.com/datasets/uciml/pima-indians-diabetes-database/data

https://www.linkedin.com/feed/?trk=guest_homepage-basic_nav-header-signin

A ideia desse projeto é criar um primeiro modelo benchmark servindo de baseline, sem a utilização de técnicas de EDA ou engenharia de variáveis. Em seguida, estabelecer um novo benchmark com o uso dessas técnicas, otimizando também os hiperparâmetros, sempre priorizando simplicidade e robustez.

Modelos utilizados:
    - Logistic Regression
    - Decision Tree Classifier
    - Random Forest Classifier
    - Support Vector Machine
    - Multilayer Perceptron
    - XGBoost

1º Benchmark (baseline)

Executando os modelos de forma simples, o foco foi não apenas a acurácia, mas principalmente o Recall da classe 1 (diabéticos). Isso porque, na maioria dos casos de diabetes ou pré-diabetes, a recomendação padrão e inicial(considerada inclusive como padrão-ouro) é a prática de exercícios físicos e a melhoria da alimentação. Ou seja, falsos positivos podem acabar se beneficiando do tratamento, já que medicamentos como insulina geralmente só são indicados após confirmação clínica mais específica, como pela dosagem da hemoglobina glicada.

Por outro lado, o falso negativo é um erro grave: dizer a uma pessoa diabética que ela não possui a doença pode agravar seu quadro e trazer sérios riscos à vida. Essa abordagem é chamada de avaliação orientada ao risco.

Diante disso, o melhor modelo foi a SVM, com 75% de acurácia e 76% de recall.

2º Benchmark (EDA e Feature Engineering)

A presença de valores zero em variáveis como glicose, insulina e pressão foi um desafio. Sabemos que é biologicamente impossível alguém estar vivo com glicose ou insulina em zero. Presume-se, portanto, que são valores ausentes ou não lidos corretamente. Testei diversos métodos de imputação, como média da amostra, KNN, entre outros, mas substituir pela mediana dos dados de treinamento foi a abordagem que gerou os melhores resultados.

Apesar da SVM ter sido o melhor modelo no benchmark inicial, os modelos baseados em árvore demonstraram robustez e estabilidade. Partindo dessa premissa, optei por criar variáveis a partir de divisões em percentis (3 ou 4) das variáveis originais, binarizando os grupos:

Um grupo com percentis que concentravam mais saídas iguais a 0.

Outro grupo com percentis que concentravam mais saídas iguais a 1.

Mesmo com as variáveis binarizadas, mantive as variáveis contínuas originais para preservar possíveis relações lineares.

Além disso, criei variáveis a partir de interações entre as originais e outras baseadas em cálculos comuns em diagnósticos de diabetes (como HOMA-IR e HOMA-BETA). O mesmo processo de percentil e binarização foi aplicado a essas novas variáveis.

Esse processo gerou alta correlação entre variáveis, mas como os modelos de árvore lidam bem com isso, decidi não remover variáveis correlacionadas — o que, aliás, teria prejudicado significativamente a performance. Como estratégia adicional, utilizei apenas 35% da base de dados para treinamento, garantindo robustez e evitando overfitting em um período de teste maior.

O escalonamento das variáveis contínuas não trouxe ganhos expressivos, mas tampouco prejudicou os resultados, e ainda ajuda a garantir uma maior estabilidade teórica dos modelos.

Resultado final - Random Forest:
Acurácia: 88%

Recall na classe 1 (diabéticos): 81%

Em casos que envolvem saúde, pequenas melhorias de performance podem ter um impacto muito relevante, uma vez que as consequências de um diagnóstico errado são potencialmente graves. É importante reforçar que a intenção de um modelo como esse não é diagnosticar, mas sim servir como um filtro prévio, incentivando pessoas em risco a procurarem um médico para diagnóstico clínico definitivo, com os exames adequados.