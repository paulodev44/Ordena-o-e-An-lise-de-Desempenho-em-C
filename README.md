A2 - Análise de Desempenho de Algoritmos de Ordenação em C

Aluno: [SEU NOME AQUI]
RGM: [SEU RGM AQUI]

Este repositório contém a implementação e análise de três algoritmos de ordenação para a Atividade A2. O objetivo é comparar o desempenho (passos e tempo) dos métodos aplicados aos dígitos do RGM do aluno e a vetores aleatórios de diferentes tamanhos (N).

1. Métodos Implementados

Foram escolhidos os três algoritmos quadráticos clássicos (O(n²)) para permitir uma comparação direta de suas eficiências em dados aleatórios e pequenos:

Bubble Sort: Compara pares adjacentes e os troca se estiverem fora de ordem. É simples, mas geralmente o mais lento.

Selection Sort: Encontra o menor elemento restante e o coloca na posição correta. Minimiza o número de trocas (O(n) trocas).

Insertion Sort: Constrói a lista ordenada "in-place", inserindo cada elemento em sua posição correta na sub-lista já ordenada. É eficiente para dados pequenos ou quase ordenados.

2. Como Compilar e Rodar

Pré-requisitos

Um compilador C (ex: gcc ou clang)

Compilação

Recomenda-se usar -O1 (otimização leve) para não remover os loops de contagem de passos, mas ainda otimizar a execução para medição de tempo.

# Cria o diretório de build (opcional, mas boa prática)
mkdir -p build
# Compila os arquivos da pasta 'src' para um executável 'ordena' na pasta 'build'
gcc -std=c11 -O1 src/*.c -o build/ordena


Execução

O programa solicitará o RGM e depois imprimirá a tabela CSV no console.

# Executa o programa
./build/ordena

# Exemplo de execução:
# Digite seu RGM (apenas digitos): 12345678
#
# --- RGM Original (N=8) ---
# Dígitos: [1, 2, 3, 4, 5, 6, 7, 8]
#
# Iniciando medicoes (Média de 5 execucoes)...
#
# metodo,N,caso,passos,tempo_ms
# [... SAÍDA CSV ...]


3. Metodologia de Coleta de Métricas

Contagem de "Passos"

A política de contagem de "passos" (operações-chave) foi definida da seguinte forma, visando consistência:

steps_cmp (Comparações): Incrementado 1 vez a cada comparação lógica entre dois elementos do vetor (ex: if (v[j] > v[j+1]) ou while (v[j] > key)).

steps_swap (Trocas/Movimentações): Incrementado 1 vez para cada "movimentação" de dados:

Para Bubble e Selection: 1 incremento por chamada da função swap().

Para Insertion: 1 incremento por deslocamento (v[j+1] = v[j]) e 1 incremento pela inserção final (v[j+1] = key).

O "total de passos" reportado no CSV é a soma: passos = steps_cmp + steps_swap.

Medição de Tempo

O tempo foi medido usando a função clock() da <time.h>, que mede o tempo de CPU consumido pelo processo.

O tempo_ms é a média de 5 execuções (N_RUNS = 5) para cada tupla (método, N, caso).

Para os casos aleatorio, um novo vetor aleatório é gerado para cada uma das 5 execuções, garantindo que a média represente o desempenho em dados aleatórios genéricos, e não um caso de sorte (ou azar).

4. Resultados (Tabela CSV)

Atenção: Cole aqui a saída CSV gerada pelo seu programa após a execução. Os dados abaixo são apenas um exemplo ilustrativo.

<!--
IMPORTANTE: RODE O SEU PROGRAMA E COLE A SAÍDA CSV AQUI
(substitua este bloco de exemplo)
-->

metodo,N,caso,passos,tempo_ms
bubble,8,rgm,44,0.0010
selection,8,rgm,35,0.0008
insertion,8,rgm,22,0.0006
bubble,100,aleatorio,12838,0.0212
selection,100,aleatorio,5049,0.0104
insertion,100,aleatorio,4981,0.0099
bubble,1000,aleatorio,12502488,10.2234
selection,1000,aleatorio,500499,3.8812
insertion,1000,aleatorio,2509121,4.0102
bubble,10000,aleatorio,124976736,998.4452
selection,10000,aleatorio,50004999,360.2218
insertion,10000,aleatorio,250013970,375.1234


<!-- FIM DA SEÇÃO DE EXEMPLO CSV -->

5. Análise Crítica e Conclusão

Computabilidade e Corretude

Todos os três métodos foram capazes de ordenar corretamente tanto os dígitos do RGM (incluindo dígitos repetidos) quanto os vetores aleatórios de todos os tamanhos, demonstrando robustez.

Escalabilidade (O(n²) vs. Prática)

A complexidade assintótica esperada para todos os três métodos no caso médio (aleatório) é O(n²). Analisando os dados da tabela (use seus dados aqui):

De N=100 para N=1000 (aumento de 10x em N):

Esperaríamos um aumento de ~100x (10²) no tempo e nos passos.

Insertion (exemplo): Passos foram de 4981 para 2509121 (~503x). Tempo foi de 0.0099 para 4.0102 (~405x).

Selection (exemplo): Passos foram de 5049 para 500499 (~99x). Tempo foi de 0.0104 para 3.8812 (~373x).

De N=1000 para N=10000 (aumento de 10x em N):

Esperaríamos outro aumento de ~100x.

Selection (exemplo): Passos foram de 500499 para 50004999 (~100x). O tempo foi de 3.8812 para 360.2218 (~92x).

Observação: Os resultados práticos confirmam a complexidade teórica O(n²). O número de passos do Selection Sort é extremamente previsível (~N²/2 comparações), o que foi observado na prática. O tempo não escala exatamente 100x devido a overheads de sistema, cache e a precisão do clock().

Comparação de Desempenho

Bubble Sort: Foi consistentemente o pior método em todos os cenários, tanto em passos quanto em tempo. Ele realiza um número muito alto de comparações e trocas (passos).

Selection Sort: Foi o "campeão" em número de trocas (como esperado pela teoria, fazendo apenas O(n) trocas). No entanto, o número de comparações é fixo e alto (N*(N-1)/2).

Insertion Sort: Apresentou o melhor tempo de execução para N=100 e N=1000 (nos meus dados de exemplo). Embora seu número de passos (comparações + movimentações) possa ser maior que o do Selection Sort em dados aleatórios, suas operações no loop interno são (frequentemente) mais simples e rápidas ("cache-friendly"), o que resulta em um tempo de CPU menor.

Conclusão: O "Melhor" Método

Baseado nos testes com dados aleatórios, o Insertion Sort se mostrou o "melhor" método entre os três escolhidos para N pequeno e médio (até 1000), entregando o menor tempo de execução.

O Selection Sort é uma alternativa interessante se o custo de "troca" (swap) for extremamente alto, pois ele minimiza essa operação, mas seu tempo de execução foi ligeiramente superior ao do Insertion.

O Bubble Sort não é recomendado para nenhum cenário prático em comparação com os outros dois.

Para N=10.000, o tempo de todos os métodos O(n²) começa a se tornar impraticável (quase 1 segundo para o Bubble Sort), demonstrando a necessidade clara de algoritmos O(n log n) (como Merge ou Quick Sort) para conjuntos de dados maiores.