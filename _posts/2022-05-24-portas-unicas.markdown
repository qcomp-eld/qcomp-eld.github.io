---
layout: post
author: 'Fernando Caletti'
title:  "Portas Quânticas Iniciais (Pauli)"
date:   2022-05-24 08:00:00
categories: teoria computacao-quântica educativo
usemathjax: true
---

Portas lógicas formam a base da computação clássica. São ferramentas utilizadas para realização de operações lógicas utilizando bits.
Quando nos referimos à computação quântica, temos um base computacional ainda em construção, com portas lógicas especificamente criadas para lidarem com Qubits (Bits quânticos).
No presente artigo serão abordadas as portas lógicas mais simples, que lidam com apenas um qubit, para aprofundarmos nosso conhecimento em computação quântica!

### Computação Clássica
Atualmente, a computação classica baseada em conceitos digitais, é amplamente utilizada no mundo todo e baseada em bits, 
que representam a menor unidade possível para armazenamento de informações digitais, possuindo dois estados possíveis: 0 ou 1.
Sistemas digitais são todos baseados na manipulação dos bits e para isso, utilizam-se da conhecida "Álgebra Booleana", onde é possível criar expressões complexas a partir de bits e operações
predefinidas que podem ser aplicadas a eles. Dentre as operações booleanas existentes, as mais conhecidas são: AND (E), OR (OU), NOT (NEGAÇÃO).

### Portas Lógicas Digitais
As portas lógicas representam aplicações fisicas das operações booleanas, que podem ser aplicadas diretamente aos bits e que podem ser construídas através de circuitos eletrônicos. As mais famosas são:
##### Porta AND (E)
Realiza uma comparação entre dois bits representada como uma multiplicação (.), ou seja, possui dois bits de entrada, e caso ambos estejam no valor 1, o resultado será 1. Caso contrário, o resultado será sempre 0.
![Porta AND](/assets/images/portas-simples/AND-Gate.png)

##### Porta OR (OU)
Realiza uma comparação entre dois bits representada como uma soma (+). Caso qualquer um dos bits de entreda possua o valor 1, a saída será 1, ou seja, a única situação possível de retornar 0 será quando os dois bits de entrada estiverem zerados.
![Porta OR](/assets/images/portas-simples/OR-Gate.png)

##### Porta NOT (NEGAÇÃO)
Realiza a inversão do valor de entrada. Possui apenas um bit de entrada e seu retorno é o valor oposto, então caso o bit de entrada possua o valo 1, a operação resultará em 0 e assim respectivamente.
![Porta NOT](/assets/images/portas-simples/NOT-Gate.png)

### O estado de um Qubit
Antes de abordar diretamente sobre as operações lógicas realizadas com qubits, precisamos definir brevemente seu estado, que precisa ser considerado de uma maneira diferente de bits clássicos. Qubits seguem diretamente as regras da mecânica quântica, permitindo assim uma gama maior e mais complexa de estados possíveis.

Enquanto bits clássicos possuem apenas seus dois estados fundamentais: 0 e 1, de forma similar, qubits também possuem os estados 0 e 1, porém, tendo em mente que a abordagem utilizada baseia-se diretamente nas leis da mecânica quântica, a superposição desses estados é permitida e eles deixam de ser determinísticos para se tornarem probabilisticos!

Dessa forma, apenas saberemos realmente o estado atual de um qubit ao medí-lo. Assim sendo, antes da medição, não podemos afirmar com certeza o valor do estado de um qubit, mas sim a probabilidade de que, ao medí-lo, determinado valor será encontrado.

##### State-Vectors
Assim sendo, pode-se representar o estado de um qubit de diversas maneiras, uma delas é a partir de vetores de estado, que é um vetor composto pelas amplitudes dos respectivos estados de um qubit. Na imagem abaixo, podemos ver o vetor de estados de um qubit nos estados 0 e 1.

![Vetores de estado](/assets/images/portas-simples/state_vectors.png)

As amplitudes, referem-se à raíz quadrada da probabilidade, para possibilitar a utilização de números negativos durante a manipulação de estados do qubits a partir de Algebra Linear. 

O sinal presente nos valores dos estados representa a "fase" do estado, que define também sua forma de interferência. Como devemos seguir à risca as leis da mecânica quântica, é possível que após determinada operação os estados em superposição se excluam mutuamente devido à interferência, por conta desse comportamento é necessária a utilização do sinal para indicação da fase. 

Como um valor negativo não faz sentido no contexto de probabilidades, utiliza-se sua raíz-quadrada, de forma que a propabilidade sempre será o quadrado da amplitude, removendo valores negativos.

![Regra da Amplitude](/assets/images/portas-simples/amplitude-rule.png)

Com a possibilidade da superposição entre os estados, podemos visualizar o estado geral de um qubit como a combinação de dois estados complexos, algebricamente cada possível estado é um número composto por uma parte real e uma imaginária, esse comportamento é expressado a partir da definição abaixo:

![Estado Algébrico](/assets/images/portas-simples/algebric_state.png)

#### Bloch Sphere
Por conta de seu comportamento probabilístico, temos que ao representar um qubit como um vetor de amplitudes, também devemos permitir que os seus componentes sejam números complexos, como mencionado anteriormente. Dessa forma, inicialmente seria impossível criar uma visão geométrica precisa do estado de um qubit, pois quatro dimensões seriam necessárias.

Tendo esse problema em mente, o físico Felix Bloch criou a Block Sphere. A Bloch sphere consiste em uma representação geométrica da imagem acima (Estado algébrico), em que passa-se a considerar um terceiro eixo como sendo a diferença de fase entre as outras duas componentes. Assim, é possível representar em 3 dimensões todas as 4 propriedades. Abaixo, podemos ver uma imagem da [Bloch Sphere](https://javafxpert.github.io/grok-bloch/):

![Block Sphere](/assets/images/portas-simples/bloch-2.png)

### Circuito Quântico
Assim como na computação clássica, na computação quântica portas lógicas vêm sendo desenvolvidas para aplicar operações pre-definidas aos qubits. Como o estado atual dos qubits é composto por números complexos representados como vetores, tem-se que as portas quânticas são equivalentes a uma determinada "Matriz de Transferência." As "Matrizes de Transferência" são desenvolvidas para aplicar determinada operação ao vetor de estado de um qubit a partir de sua multiplicação algébrica e assim obter .
Atualmente, algoritmos quânticos ainda são desenvolvidos no formato de circuitos, em que possuimos qubits de entrada e podemos aplicar portas quânticas em suas trilhas para alterar seu comportamento. Ao final do circuito, deve-se realizar a medição do valor atual do qubit, na imagem abaixo, pode-se ver um exemplo de circuito quântico:
(Para os exemplos nesse artigo, foram utilizadas imagens da documentação Qiskit e do IBM Quantum Composer)

![Circuito Quântico](/assets/images/portas-simples/quantum_circuit.png)

#### Portas de Pauli
Dentre as portas quânticas já desenvolvidas, as mais simples são aquelas que possuem apenas 1 qubit de entrada e uma saída. Nesse artigo serão abordadas as portas de Pauli, que são a porta de entrada para o estudo das portas quânticas existentes.

##### Porta X (NOT)
A porta X é constantemente relacionada com a porta digital NOT, pois ela é capaz de inverter o estado de um qubit. Na imagem abaixo, pode-se visualizar sua matriz de transferência.
![X-Gate](/assets/images/portas-simples/x_matrix.png)

Para realizarmos a aplicação da matriz de transferência no qubit, realizamos uma simples multiplicação matricial a partir de seu vetor de estado e a matriz, como podemos visualizar na imagem abaixo:
![Gate-Product](/assets/images/portas-simples/Matrix-multiplication.png)

Aplicando a mesma operação com uma porta X, temos:
![X-Gate-Calc](/assets/images/portas-simples/x_matrix_calc.png)

Sendo assim, podemos perceber que a porta X tem a capacidade de inverter diretamente o valor do qubit, porém também pode-se visualizar como a aplicação de uma movimentação Pi no eixo X da bloch sphere, como podemos perceber na imagem abaixo:
![X-Gate-Calc](/assets/images/portas-simples/Bloch-X.png)

Exemplos utilizando o IBM Quantum Composer:
![Composer-X](/assets/images/portas-simples/Composer-X.png)

##### Portas Y e Z
As portas Y e Z possuem características muito similares à porta X, também geram rotações por Pi, porém nos eixos Y e Z da Bloch Sphere respectivamente. Na imagem abaixo pode-se visualizar as respectivas matrizes de transferência:
![Y-And-Z-Gates](/assets/images/portas-simples/Y and Z.png)

Como mencionado anteriormente, a rotação na bloch sphere ocorre nos eixos Y e Z, como representado abaixo:
![Y-Gate-Rotation](/assets/images/portas-simples/Bloch-Y.png)
![Z-Gate-Rotation](/assets/images/portas-simples/Bloch-Z.png)

Exemplos utilizando o IBM Quantum Composer:
![Composer-Y](/assets/images/portas-simples/Composer-Y.png)
![Composer-Z](/assets/images/portas-simples/Composer-Z.png)

##### Porta I
A porta I possui sua matriz de transferência similar às demais, porém com a peculiaridade de deixar a entrada intacta, sem aplicar nenhum tipo de alteração. A porta I, também conhecida por "Identidade" acaba por ser util ao realizar arranjos com expressões complexas e também para ser implementado um hardware que respectivamente apenas disponibilize um sinal exatamente igual ao recebido em sua saída. A respectiva matriz de transferência pode ser visualizada na imagem abaixo:
![I-Gate](/assets/images/portas-simples/Pauli-I-c.png)

Exemplos utilizando o IBM Quantum Composer:
![Composer-H-0](/assets/images/portas-simples/Composer-H-0.png)

#### Superposição
A superposição acaba por ser a base para a utilidade prática da computação quântica e um dos principais motivos para que ela seja considerada de extrema importância para o futuro. A ideia de superposição segue estritamente as regras da mecânica quântica, definindo que um qubit pode estar em nos dois estados ao mesmo tempo, isso é possível pois devemos ter em mente que, nesse caso, o estado refere-se à probabilidade de um determinado estado real ser encontrado naquele momento, isso implica que o estado atual do qubit não é determinístico, mas sim probabilístico e é representado por uma expressão complexa de coeficientes multiplicados pelos estados possíveis, como demonstrado acima. A utilidade prática de um computador quântico vem à tona quando imaginamos esse tipo de informação sendo tratada de forma nativa por um hardware, da forma que bits são tratados por equipamentos digitais, assim temos a possibilidade de realizar cálculos e aproximações complexas, como a simulação de uma molécula, apenas correlacionando estados de qubits em superposição.
Até o momento, todas as portas de Pauli demonstradas não necessariamente estão considerando esse efeito, pois elas apenas rotacionam o valor de um qubit, independente de seu estado atual, então se partirmos do pressuposto que estamos utilizando o qubit que inicia no estado 0, utilizando apenas as portas de Pauli, permanecemos com respostas determinísticas na saída, pois apenas rotacionando a partir do estado 0 ainda pode-se saber com certeza qual estado será atingido.

##### Porta Hadamard
Para poder gerar o comportamento de superposição no estado de um qubit, utiliza-se a porta de Hadamard, sua matriz de transferência pode ser visualizada abaixo:
![H-Gate](/assets/images/portas-simples/H-Matrix.png)

Ao aplicar-se a porta Hadamard podemos ver os seguintes resultados:
![H-States](/assets/images/portas-simples/H-States.png)

Que são notações definidas para os estados:
![Superposition-States](/assets/images/portas-simples/Superposition-states.png)

Exemplos utilizando o IBM Quantum Composer:
![Composer-H-0](/assets/images/portas-simples/Composer-H-0.png)
![Composer-H-1](/assets/images/portas-simples/Composer-H-1.png)
















