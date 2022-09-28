---
layout: post
title:  "Lista de Operações do IBM Quantum Composer"
date:   2022-09-27 00:00:00
author: fernando_caletti
categories: teoria computacao-quântica educativo
usemathjax: true
excerpt_separator: <!--more-->
---

Até o presente momento, viemos utilizando com frequência os simuladores disponibilizados pela IBM para simulações e demonstrações durante os estudos de outros tópicos na área de computação quântica. Com o intuito de prover um entendimento maior sobre a ferramenta que tem se provado bem útil para os estudos na área, o presente artigo visa servir como um resumo de todas as operações disponibilizadas na ferramenta IBM Quantum Composer, com exemplos práticos. Para que dessa forma, possamos passar para uma abordagem mais prática no estudo, a partir do uso mais frequente de ferramentas de simulação como essa.


<!--more-->

### O IBM Quantum Composer
IBM Quantum Composer é uma ferramenta de simulação já utilizada diversas vezes aqui no blog de computação quântica do ELDORADO. A ferramenta disponibiliza uma visualização dos algoritmos em formato de circuitos quânticos, com a possibilidade de arrastar blocos de operações para serem aplicadas aos qubits. Além disso, diferentes métricas são colhidas diretamente sobre a saída do algoritmo implementado em tempo real, como: Seu gráfico de probabilidades de estado na saída e a representação visual do estado do sistema a partir de uma Bloch Sphere.
Juntamente a tudo isso, a ferramenta ainda disponibiliza uma aba lateral, que permite visualizar o código gerado automaticamente ou fazer modificações no circuido gerado a partir de seu código. A visualização de código pode ser apresentada em duas linguagens: OPENQASM 2.0 e Qiskit (python).
É importante ressaltar que a edição do circuito via código, no presente momento (27/09/2022), só é possível a partir da linguagem OPENQASM 2.0. Ao passar para a visualização do código em Qiskit, o editor se torna apenas-leitura e é possível apenas visualizar os códigos gerados automaticamente.

### Lista de Operações do Quantum Composer
Como pode-se visualizar na imagem abaixo, a ferramenta já conta com uma gama interessante de operações que podem ser aplicadas nos Qubits. Na documentação [oficial da ferramenta](https://quantum-computing.ibm.com/composer/docs/iqx/operations_glossary), as operações permitidas são divididas em 5 classificações distintas, elas são: Operações clássicas, Operações de fase, Operações não unitárias e modificadores, Hadamard, Operações quânticas.


![Operações Quantum Composer](/assets/images/portas-composer/All-operations-composer.png)

#### Operações Clássicas
Iniciaremos a revisão com as operações clássicas, que podem ser aplicadas a qubits sem necessáriamente estarem em um estado de superposição.
##### Operação NOT (Pauli X)
A operação NOT, conhecida como porta X de Pauli, já abordada no artigo [portas quânticas iniciais]({% post_url 2022-05-24-portas-unicas %}), possui a capacidade de inverter o estado de um qubit, como demonstrado abaixo: 


$$ \ket{0} \rightarrow \ket{1} $$  
$$ \ket{1} \rightarrow \ket{0} $$

Sua matriz de transferência está demonstrada abaixo:

$$ 
X = \left[ {\begin{array}{cc}
0 & 1 \\
1 & 0\\
\end{array} } \right] 
$$

A operação NOT é a equivalência de uma operação RX para o ângulo $$\pi$$ ou para 'HZH'.

Na imagem abaixo podemos visualizar a referência da operação na ferramenta Quantum Composer:


![Descrição X](/assets/images/portas-composer/X-composer-description.png)

##### Operação CNOT
A operação CNOT, ou Controlled-Not (Not controlado), também conhecida como CX, age em um par de qubits, sendo o primeiro deles admitido como qubit de 'controle' e o segundo como 'alvo'. A operação consiste em aplicar uma lógica NOT ao qubit 'alvo' sempre que o qubit 'controle' estiver no estado $$\ket{1}$$. Se o qubit 'controle' estiver em superposição, a operação CNOT gera entrelaçamento (entanglement) entre os qubits.

De forma simplificada, todos os circuitos unitários podem ser decompostos em operações simples em apenas um qubit e operações CNOT. Por conta do tempo de execução de uma operação CNOT ser muito maior do que o tempo necessário para execução de operações de apenas um qubit em hardwares reais, o custo de processamento de um circuito é geralmente medido pelo número de operações CNOT que ele contém.

A matriz de transferência da operação CNOT está apresentada abaixo:

$$ 
CX = \left[ {\begin{array}{cc}
1 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 \\
0 & 0 & 0 & 1 \\
0 & 0 & 1 & 0 \\
\end{array} } \right] 
$$

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição CNOT](/assets/images/portas-composer/CNOT-composer-description.png)

##### Operação Toffoli
A operação Toffoli também é conhecida como: CNOT com duplo controle (CCX). Pois possue dois qubits 'controle' e apenas um qubit 'alvo'. A operação aplica uma lógica NOT ao qubit 'alvo' apenas quando ambos os qubits 'controle' estiverem no estado $$\ket{1}$$.

A operação Toffoli em conjunto com uma operação Hadamard é tida como uma operação universal para a computação quântica. Sua matriz de transferência está demonstrada abaixo:

$$ 
CCX = \left[ {\begin{array}{cc}
1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 \\
0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 \\
\end{array} } \right] 
$$

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição Toffoli](/assets/images/portas-composer/Toffoli-composer-description.png)

##### Operação SWAP
A operação SWAP realizada a troca dos estados de dois qubits. Sua matriz de transferência pode ser visualizada abaixo:

$$ 
SWAP = \left[ {\begin{array}{cc}
1 & 0 & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 1 & 0 & 0 \\
0 & 0 & 0 & 1 \\
\end{array} } \right] 
$$

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição SWAP](/assets/images/portas-composer/SWAP-composer-description.png)

##### Operação Identidade
A operação identidade (também conhecida como 'Porta I'), acaba por funcionar na realidade como a ausência de uma operação. A principal função da operação identidade é garantir que nada seja aplicado sobre um qubit durante uma unidade de tempo.
Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição I](/assets/images/portas-composer/I-composer-description.png)

#### Operações de Fase
##### Operação T
A operação T é a equivalente à operação RZ para o ângulo $$\frac{\pi}{4}$$. Acredita-se que computadores quânticos tolerantes a falhas irão compilar todos os programas quânticos para operações T e seu inverso, juntamente com operações Clifford (Clifford??).
Sua matriz de transferência pode ser visualizada abaixo:

$$ 
T = \left[ {\begin{array}{cc}
1 & 0 \\
0 & e^{i\pi/4}\\
\end{array} } \right] 
$$

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição T](/assets/images/portas-composer/T-composer-description.png)

##### Operação S
A operação S aplica uma fase $$i$$ para o estado $$\ket{1}$$. É o equivalente a uma operação RZ para o ângulo $$\frac{\pi}{2}$$. Perceba que: $$ S=P(\frac{\pi}{2}) $$.
A matriz de transferência da operação pode ser visualizada abaixo:

$$
S = \left[ {\begin{array}{cc}
1 && 0 \\
0 && i \\
\end{array} } \right]
$$

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição S](/assets/images/portas-composer/S-composer-description.png)

##### Operação Z
A operação Z, também conhecida como porta Z de Pauli, opera como uma identidade no estado $$\ket{0}$$ e multiplica o sinal do estado $$\ket{1}$$ por $$-1$$. Assim sendo, a operação é capaz de inverter os estados $$\ket{-}$$ e $$\ket{+}$$. Considerando os estados +/- como base, a operação Z age da mesma forma que a operação NOT sobre a base de estados 0/1.
A matriz de transferência da operação pode ser visualizada abaixo:

$$
Z = \left[ {\begin{array}{cc}
1 && 0 \\
0 && -1 \\
\end{array} } \right]
$$

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição Z](/assets/images/portas-composer/Z-composer-description.png)

##### Operação T-dagger (Hermitian)
É a operação inversa da operação T. Conhecida como Tdg.
A matriz de transferência da operação pode ser visualizada abaixo:

$$
T^\dagger = \left[ {\begin{array}{cc}
1 & 0 \\
0 & e^{-i\pi/4}\\
\end{array} } \right]
$$

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição Tdg](/assets/images/portas-composer/Tdg-composer-description.png)

##### Operação S-dagger (Hermitian)
É a operação inversa da operação S. Conhecida como Sdg.
A matriz de transferência da operação pode ser visualizada abaixo:

$$
S^\dagger = \left[ {\begin{array}{cc}
1 && 0 \\
0 && -i \\
\end{array} } \right]
$$

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição Sdg](/assets/images/portas-composer/Sdg-composer-description.png)

##### Operação Fase
A operação fase, chamada anteriormente de operação U1, aplica uma fase de $$e^{i\theta}$$ para o estado $$\ket{1}$$. Para certos valores de $$\theta$$, é o equivalente a outras operações. Por exemplo: 

$$ 
P(\pi) = Z \\
P(\pi/2) = S \\
P(\pi/4) = T
$$

Assim como, uma fase global de $$e^{i\theta/2}$$, é equivalente a uma aplicação de $$RZ(\theta)$$.
A matriz de transferência da operação pode ser visualizada abaixo:

$$
P(\lambda) = \left[ {\begin{array}{cc}
1 && 0 \\
0 && e^{i\lambda} \\
\end{array} } \right]
$$

Na ferramenta IBM Quantum Composer, o valor padrão para teta é $$ \pi/2 $$.
Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição P](/assets/images/portas-composer/P-composer-description.png)

##### Operação RZ
A operação RZ implementa $$exp(-i\frac{\theta}{2}Z)$$. Na Bloch sphere, essa operação corresponde a rotacionar o estado do qubit no eixo-z pelo ângulo especificado.
A matriz de transferência da operação pode ser visualizada abaixo:

$$
RZ(\lambda) = exp\left(-i\frac{\lambda}{2}Z\right) = \left[ {\begin{array}{cc}
e^{-i\frac{\lambda}{2}} && 0 \\
0 && e^{i\frac{\lambda}{2}} \\
\end{array} } \right]
$$

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição RZ](/assets/images/portas-composer/RZ-composer-description.png)

#### Operações Não-unitárias e Modificadores
##### Operação Reset
Retorna um qubit ao estado $$\ket{0}$$, independente do estado antes da aplicação da operação.
Não é uma operação reversível.

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição Reset](/assets/images/portas-composer/Reset-composer-description.png)

##### Medição
A operação de medição na base padrão, também conhecida como base Z ou base computacional. Pode ser usada para implementar qualquer tipo de medição quando combinada com outras operações. 
Não é uma operação reversível.

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição Medição](/assets/images/portas-composer/Measure-composer-description.png)

##### Modificação de Controle
Um modificador de controle proporciona a uma outra operação a característica de controlar sua execução a partir de um outro qubit. O modificador de controle pode ser adicionado em outras operações dentro do IBM Quantum Composer, e quando é feito, é necessário selecionar qual será o qubit de controle. Quando o estado do qubit de controle for $$\ket{1}$$, a operação que recebeu o modificador de controle será executada (em seu respectivo qubit).
Caso o qubit de controle esteja em superposição, o resultado ocorre seguindo as regras de linearidade, gerando diferentes estados possíveis.

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição Controle](/assets/images/portas-composer/Control-composer-description.png)

##### Operação IF
A operação IF permite que operações quânticas sejam aplicadas de forma condicional, dependendo do estado de um registrador clássico. Dessa forma é possível relacionar o valor de registradores clássicos ao longo da execução de algorítmos quânticos.

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição IF](/assets/images/portas-composer/If-composer-description.png)

##### Barreira
Para fazer com que um programa quântico se torne mais eficiente, o compilador tentará combinar operações. A barreira é uma instrução para o compilador evitar fazer essas combinações. De forma adicional, acaba sendo útil para visualização do algorítmo.

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição Barreira](/assets/images/portas-composer/Barrier-composer-description.png)

#### Hadamard
A operação Hadamard, ou popularmente conhecida como H, rotaciona or estados $$\ket{0}$$ e $$\ket{1}$$ para $$\ket{+}$$ e $$\ket{-}$$, respectivamente. É extremamente útil para criar superposições. Se você possuir uma operação universal na computação clássica, ao adicionar uma operação Hadamard nela voce terá uma operação universal na computação quântica.

A matriz de transferência da operação pode ser visualizada abaixo:

$$
H = \frac{1}{\sqrt{2}}\left[ {\begin{array}{cc}
1 && 1 \\
1 && -1 \\
\end{array} } \right]
$$

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição H](/assets/images/portas-composer/H-composer-description.png)

#### Operações Quânticas
##### Operação $$\sqrt{X}$$ (root-square x)
Também conhecida como operação raíz quadrada NOT. A operação implementa a raíz quadrada da operação X. Aplicando essa operação duas vezes em uma linha produz uma operação X padrão. Como a operação Hadamard, $$\sqrt{X}$$ cria um estado de superposição igual ao de um qubit no estado $$\ket{0}$$, porém com uma diferença relativa de fase. Em alguns hardwares, há uma operação (porta) nativa que pode ser implementada com um pulso de $$\pi/2$$ ou X90.

A matriz de transferência da operação pode ser visualizada abaixo:

$$
\sqrt{X} = \frac{1}{2}\left[ {\begin{array}{cc}
1 + i && 1 - i \\
1 - i && 1 + i \\
\end{array} } \right]
$$

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição Raíz X](/assets/images/portas-composer/RootX-composer-description.png)

##### Operação $$\sqrt{X}^\dagger$$ (root-square x hermitian)
Também conhecida como SXdg. É a operação inversa à $$\sqrt{X}$$. Aplicando essa operação duas vezes em uma linha, será produzida uma operação X padrão, já que a operação NOT é seu próprio inverso. Assim como a operação $$\sqrt{X}$$, a presente operação pode ser utilizada para criar um estado de superposição igual, assim como, também está implementado de forma nativa em alguns hardwares a partir do uso de um pulso X90.

A matriz de transferência da operação pode ser visualizada abaixo:

$$
\sqrt{X}^\dagger = \frac{1}{2}\left[ {\begin{array}{cc}
1 - i && 1 + i \\
1 + i && 1 - i \\
\end{array} } \right]
$$

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição Raíz X dg](/assets/images/portas-composer/RootXdg-composer-description.png)

##### Operação Y
A operação Y de Pauli é equivalente à operação RY para o ângulo $$\pi$$. É equivalente a uma aplicação de operações X e Z até um fator de fase global.

A matriz de transferência da operação pode ser visualizada abaixo:

$$
Y = \frac{1}{2}\left[ {\begin{array}{cc}
0 && -i \\
i && 0 \\
\end{array} } \right]
$$

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição Y](/assets/images/portas-composer/Y-composer-description.png)

##### Operação RX
A operação RX implementa $$exp(-i\frac{\theta}{2}X)$$. Na Bloch sphere, essa operação corresponde à rotacionar o estado de um qubit ao longo do eixo-x, a partir de um ângulo especificado.

A matriz de transferência da operação pode ser visualizada abaixo:

$$
RX(\theta) = exp\left(-i\frac{\theta}{2}X\right) = \left[ {\begin{array}{cc}
\cos{\frac{\theta}{2}} && -i\sin{\frac{\theta}{2}} \\
-i\sin{\frac{\theta}{2}} && \cos{\frac{\theta}{2}} \\
\end{array} } \right]
$$

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição RX](/assets/images/portas-composer/RX-composer-description.png)
Na ferramenta IBM Quantum Composer, o ângulo padrão utilizado é $$\pi/2$$. Assim como, esse mesmo ângulo foi usado na demonstração da imagem acima.

##### Operação RY
A operação RY implementa $$exp(-i\frac{\theta}{2}Y)$$. Na Bloch sphere, essa operação corresponde à rotacionar o estado de um qubit ao longo do eixo-y, a partir de um ângulo especificado.

A matriz de transferência da operação pode ser visualizada abaixo:

$$
RY(\theta) = exp\left(-i\frac{\theta}{2}Y\right) = \left[ {\begin{array}{cc}
\cos{\frac{\theta}{2}} && -\sin{\frac{\theta}{2}} \\
\sin{\frac{\theta}{2}} && \cos{\frac{\theta}{2}} \\
\end{array} } \right]
$$

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição RY](/assets/images/portas-composer/RY-composer-description.png)
Na ferramenta IBM Quantum Composer, o ângulo padrão utilizado é $$\pi/2$$. Assim como, esse mesmo ângulo foi usado na demonstração da imagem acima.

##### Operação RXX
A operação RXX implementa $$exp(-i\frac{\theta}{2}X\otimes X)$$. A operação Mølmer–Sørensen, que é a operação nativa em sistemas de armadilha de íons, pode ser expressada como uma soma de operações RXX.

A matriz de transferência da operação pode ser visualizada abaixo:

$$
RXX(\theta) = exp\left(-i\frac{\theta}{2}X \otimes X\right) = \left[ {\begin{array}{cc}
\cos{\frac{\theta}{2}} && 0 && 0 && -i\sin{\frac{\theta}{2}} \\
0 && \cos{\frac{\theta}{2}} && -i\sin{\frac{\theta}{2}} && 0 \\
0 && -i\sin{\frac{\theta}{2}} && \cos{\frac{\theta}{2}} && 0 \\
-i\sin{\frac{\theta}{2}} && 0 && 0 && \cos{\frac{\theta}{2}} \\
\end{array} } \right]
$$

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição RXX](/assets/images/portas-composer/RXX-composer-description.png)
Na ferramenta IBM Quantum Composer, o ângulo padrão utilizado é $$\pi/2$$.

##### Operação RZZ
A operação RZZ necessita de apenas um parâmetro: um ângulo expresso em radianos. É a representação de uma soma direta de duas operações Z, aplicada em dois qubits, gerando uma rotação Z baseado nos dois qubits de entrada. Essa operação é simétrica, ou seja, a ordem dos qubits em que é aplicada não faz diferença no resultado.

A matriz de transferência da operação pode ser visualizada abaixo:

$$
RZZ(\theta) = exp\left(-i\frac{\theta}{2}Z \otimes Z\right) = \left[ {\begin{array}{cc}
e^{-i\frac{\theta}{2}} && 0 && 0 && 0 \\
0 && e^{i\frac{\theta}{2}} && 0 && 0 \\
0 && 0 && e^{i\frac{\theta}{2}} && 0 \\
0 && 0 && 0 && e^{-i\frac{\theta}{2}} \\
\end{array} } \right]
$$

Na imagem abaixo podemos visualizar a referência da operação na ferramenta Quantum Composer:


![Descrição RZZ](/assets/images/portas-composer/RZZ-composer-description.png)
Na ferramenta IBM Quantum Composer, o ângulo padrão utilizado é $$\pi/2$$.


##### Operação U
Anteriormente chamada de operação U3, a partir de três parâmetros é possível construir qualquer operação para apenas um qubit a partir da uma operação U. Possui a duração de uma unidade de tempo. 

A matriz de transferência da operação pode ser visualizada abaixo:

$$
U(\theta, \phi, \lambda) = \left[ {\begin{array}{cc}
\cos{\frac{\theta}{2}} && -e^{i\lambda}\sin{\frac{\theta}{2}} \\
e^{i\phi}\sin{\frac{\theta}{2}} && e^{i(\phi + \lambda)}\cos{\frac{\theta}{2}} \\
\end{array} } \right]
$$

Na imagem abaixo podemos visualizar a referência da operação na ferramenta Quantum Composer:


![Descrição U](/assets/images/portas-composer/U-composer-description.png)
Na ferramenta IBM Quantum Composer, o ângulo padrão utilizado é $$\pi/2$$.

##### Operação RCCX
É a simplificação da operação Toffoli, também conhecida como operação Margolus. A simplificação da operação Toffoli implementa a operação Toffoli a partir de fases relativas. Essa implementação precisa de três operações CX, o que é a menor quantidade possível, como demonstrado [aqui](https://arxiv.org/abs/quant-ph/0312225). Perceba que a simplificação de Toffoli não é equivalente a uma operação Toffoli, mas pode ser utilizada em momentos onde a operação Toffoli não precisaria ser computada novamente.

Na imagem abaixo podemos visualizar a referencia da operação na ferramenta Quantum Composer:

![Descrição RCCX](/assets/images/portas-composer/RCCX-composer-description.png)

##### Operação RC3X
É a simplificação de uma operação Toffoli controlada em três.
A simplificação Toffoli implementa a operação Toffoli com fases relativas. Assim como a operação RCCX, não é uma equivalente à operação Toffoli, mas pode ser usada em momentos em que a operação completa não precisa ser computada novamente.

Na imagem abaixo podemos visualizar a referência da operação na ferramenta Quantum Composer:


![Descrição RC3X](/assets/images/portas-composer/RC3X-composer-description.png)

-------
#### Operações Obsoletas
Abaixo está representada uma lista de operações que não estão mais disponíveis na ferramenta IBM Quantum Composer, porém ainda são referenciadas na documentação para fins de histórico.
- CSWAP
- U1
- U3
- U2
- CU1
- CU3
- CH
- CY
- CZ
- CRX
- CRY
- CRZ

-------

### Conclusão
O presente artigo tem por principal objetivo descrever as funcionalidades atuais da ferramenta IBM Quantum Composer em um guia de fácil acesso, para que a ferramenta possa ser utilizada de forma mais práticas. As informações contidas neste material foram completamente baseadas na documentação oficial da ferramenta, que pode ser lida [aqui](https://quantum-computing.ibm.com/composer/docs/iqx/operations_glossary#u-gate) e na documentação oficial da bilbioteca [Qiskit](https://qiskit.org/documentation/stubs/qiskit.circuit.library.RC3XGate.html).
Esperamos que o material seja útil para seus estudos e experimentos!

### Referências
- https://quantum-computing.ibm.com/composer/docs/iqx/operations_glossary

- https://qiskit.org/documentation/stubs/qiskit.circuit.library.RC3XGate.html
- https://arxiv.org/abs/quant-ph/0312225
- https://www.ibm.com/blogs/research/2019/12/qiskit-openpulse/