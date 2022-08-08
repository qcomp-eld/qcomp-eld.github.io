---
layout: post
title:  "Múltiplos qubits, produto tensorial e entrelaçamento quântico"
date:   2022-08-06 12:13:00
author: alisson_fumaco
categories: matemática computacao-quântica produto-tensorial entrelaçamento
usemathjax: true
excerpt_separator: <!--more-->
---

<!--more-->

Como visto em posts anteriores, o bloco fundamentar da computação quântica é o qubit, que é uma combinação linear de $$ \ket{0} $$ e $$ \ket{1} $$.

$$ \psi = a\ket{0} + b\ket{1} $$

Ou se preferir.

$$
\psi = \left[ {\begin{array}{cc}
a \\
b \\
\end{array} } \right]
$$

Um computar de um único qubit não é muito útil. São necessários múltiplos qubits para ser possível resolver algum problema real.

### Múltiplos qubits
Por simplicidade, vamos trabalhar com um sistema de apenas dois qubits.

Provavelmente, a primeira ideia que vem a sua mente é montar o seguinte sistema linear.

$$
\psi = a_1\ket{0} + b_1\ket{1}
$$
$$
\phi = a_2\ket{0} + b_2\ket{1}
$$

onde $$\psi$$ é o vetor de estados do primeiro qubit e $$\phi$$ é o vetor estado do segundo qubit.

Apesar de essa ser a representação mais intuitiva, a forma mais usada para se trabalhar com esses sistemas é através de um vetor de estados.

$$
\ket{\psi\phi} = \ket{\psi} \otimes \ket{\phi} = \alpha_1\ket{00} + \alpha_1\ket{01} + \alpha_1\ket{10} + \alpha_1\ket{11}
$$

Ou como vetor.

$$
\ket{\psi\phi} = \left[ {\begin{array}{cc}
\alpha_1 \\
\alpha_2 \\
\alpha_3 \\
\alpha_4 \\
\end{array} } \right]
$$

Já que ambas as representações estão corretas. Deve existir uma relação entre elas. Assim, como que os `a`s e `b`s se relacionam com os alfas ($$\alpha$$)?

Vamos fazer uma coisa perigosa e assumir que a mecânica quântica faz sentido. Assumindo isso, podemos criar um diagrama de árvore de probabilidade como o da figura a seguir.

![](/assets/images/multiplos-qubits/exampl1.png)

Percorrendo o caminho até chegar ao estado $$\ket{00}$$, vemos que a probabilidade é dada por $$a_1a_2$$. E fazendo o mesmo para os demais, podemos perceber que $$\ket{01} = a_1b_2$$, $$\ket{10} = b_1a_2$$ e $$\ket{11} = b_1b_2$$. Ou seja,

$$
\ket{\psi\phi} = \left[ {\begin{array}{cc}
\alpha_1 \\
\alpha_2 \\
\alpha_3 \\
\alpha_4 \\
\end{array} } \right]
= 
\left[ {\begin{array}{cc}
a_1a_2 \\
a_1b_2 \\
b_1a_2 \\
b_1b_2 \\
\end{array} } \right]
$$

Então temos uma maneira de representar os possíveis estados de um sistema de dois qubits. Porém, dado dois qubits, digamos $$\ket{0}$$ e $$\ket{1}$$ como que chegamos nesse vetor de estados?

### Produto tensorial
Para representar o vetor de estados de dois, ou mais, qubits, usamos o produto tensorial, mas o que é um produto tensorial exatamente?

Vamos começar definindo o produto tensorial entre dois qubits. Então, o produto tensorial entre dois qubits $$\ket{a}=(a_1,b_1)$$ e $$\ket{b}=(a_2, b_2)$$ é dado através da seguinte fórmula

$$
a \otimes b = \left[ {\begin{array}{cc}
a_1a_2 \\
a_1b_2 \\
b_1a_2 \\
b_1b_2 \\
\end{array} } \right]
$$


Para termos um exemplo com números, vamos calcular o produto tensorial $$ \ket{0} \otimes \ket{+} $$.

$$ \ket{0} \otimes \ket{+}  $$

$$
\ket{0} \otimes \ket{+} =  

\left[ {\begin{array}{cc}
1 \\
0 \\
\end{array} } \right]

\otimes

\frac{1}{\sqrt{2}} \left[ {\begin{array}{cc}
1 \\
1 \\
\end{array} } \right]

=  

\frac{1}{\sqrt{2}} \left[ {\begin{array}{cc}
1 \\
1 \\
0 \\
0 \\
\end{array} } \right]
$$

Como toda operação matemática, temos algumas propriedades que são amplamente utilizadas:
1. $$(\alpha \ket{v}) \otimes \ket{w} = \ket{v} \otimes (\alpha  \ket{w}) = \alpha(\ket{v} \otimes \ket{w})$$
2. $$(\ket{v} \otimes \ket{w}) \otimes \ket{u} = \ket{v} \otimes (\ket{w} \otimes \ket{u})$$
3. $$(\ket{v_1} + \ket{v_2}) \otimes \ket{w} = \ket{v_1} \otimes \ket{w} + \ket{v_2} \otimes \ket{w}$$
4. $$\ket{v} \otimes (\ket{w_1} + \ket{w_2}) = \ket{v} \otimes \ket{w_1} + \ket{v} \otimes \ket{w_2}$$

**Importante**: as operações de produto tensorial não são comutativas, isso é, $$\ket{v} \otimes \ket{w} \neq \ket{w} \otimes \ket{v}$$.

Então no final, podemos escrever um sistema de dois qubits das seguintes formas

$$
\ket{01} = \ket{0} \otimes \ket{1}
$$

E um exemplo mais interessante, seria um dos bell state.

$$
 \ket{\Phi^+} = \frac{1}{\sqrt{2}}(\ket{0} \otimes \ket{0} + \ket{1} \otimes \ket{1}) = \frac{1}{\sqrt{2}}(\ket{00} + \ket{11})
$$

Essa definição também pode ser estendida a matrizes:

$$
\left[ {\begin{array}{cc}
a_1 \\
a_2 \\
\end{array} } \right] \otimes \left[ {\begin{array}{cc}
b_1 & b_2 \\
b_3 & b_4 \\
\end{array} } \right]

=

\left[ {\begin{array}{cc}
a_1 \otimes \left[ {\begin{array}{cc}
b_1 & b_2 \\
b_3 & b_4 \\
\end{array} } \right] \\

a_2 \otimes \left[ {\begin{array}{cc}
b_1 & b_2 \\
b_3 & b_4 \\
\end{array} } \right]

\end{array} } \right]

=

\left[ {\begin{array}{cc}
a_1b_1 & a_1b_2 \\
a_1b_3 & a_1b_4 \\
a_2b_1 & a_2b_2 \\
a_2b_3 & a_2b_4 \\
\end{array} } \right]
$$

### Entrelaçamento
Como vimos na seção anterior, podemos representar um sistema de qubits através de um produto tensorial.

Porém, uma pergunta que fica no ar é: dado o vetor resultado do produto tensorial, podemos encontrar dois vetores que formam ele?

Por exemplo, dado o seguinte vetor de estados de dois qubits. É possível encontrar dois qubits onde o produto tensorial resulta no mesmo?

$$
\ket{\Phi^+} = \frac{1}{\sqrt{2}} \left[ {\begin{array}{cc}
1 \\
0 \\
0 \\
1 \\
\end{array} } \right]
$$

A resposta é não.

Se um sistema de qubits $$\ket{\Psi}$$ não pode ser escrito como $$\ket{\psi} \otimes \ket{\phi}$$. Então dizemos que existe um entrelaçamento quântico e quando podemos escrever o sistema através de um produto tensorial, dizemos ser um estado separável.

Uma dúvida que fica no ar, é como que nós geramos um entrelaçamento quântico como o $$\ket{\Phi^+}$$?

A forma mais simples é usando um Hadamard gate e um CNOT gate como mostrado na figura abaixo.

![Circuito para gerar um bell state](/assets/images/multiplos-qubits/bell-state-circuit.png)
<sub><sup>*Ref: https://en.wikipedia.org/wiki/Bell_state#/media/File:The_Hadamard-CNOT_transform_on_the_zero-state.png </sup></sub>

No sistema de dois qubits, primeira aplicamos o hadamard gate ao primeiro qubit (H na imagem acima). Isso vai gerar uma sobreposição.

$$
\frac{( \ket{0} + \ket{1})\ket{0}}{\sqrt{2}} = \ket{+0}
$$

Então usamos essa sobreposição como controlador do CNOT no segundo qubit, e assim, gerando o bell state $$\ket{\Phi^+}$$.


### Dicas de Leitura
Para um passo a passo mais detalhado de como gerar o bell state, e como gerar os demais bell states, eu recomendo o post [truth tables with grover's search in qiskit](https://quantumcomputinguk.org/tutorials/introduction-to-bell-states) do quantum computing uk.

Durante o post  do quantum computing uk é apresentado a seguinte matrix como sendo o operador CNOT.
$$
CNOT = \left[ {\begin{array}{cc}
1 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 \\
0 & 0 & 0 & 1 \\
0 & 0 & 1 & 0 \\
\end{array} } \right]
$$
Porém, não é justificado a origem dessa matrix. O usuário Perry Sakkaris proveu uma boa explicação no [Stack Exchange](https://quantumcomputing.stackexchange.com/a/5192).

É possível encontrar uma introdução mais teórica sobre produto tensorial no canal [Mu Prime Math](https://www.youtube.com/watch?v=KnSZBjnd_74).

### Referências
- Eleanor Rieffel and Wolfgang Polak. 2011. Quantum Computing: A Gentle Introduction (1st. ed.). The MIT Press.
- https://quantumcomputinguk.org/tutorials/introduction-to-bell-states
- https://en.wikipedia.org/wiki/Bell_state