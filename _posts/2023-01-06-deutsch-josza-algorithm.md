---
layout: post
title:  "Algoritmo de Deutsch-Josza"
date:   2023-01-06 10:00:00
author: luccas_marim
categories: teoria computacao-quântica educativo
usemathjax: true
---

Neste artigo iremos introduzir, a partir de [ref.I], um dos primeiros algoritmos quânticos criados. Embora não tenha muita aplicação prática, este é simples e fornece um ótimo exemplo de como trabalhar com computação quântica exclusivamente em teoria, o que, de fato, é útil estando na era NISQ. Conforme [ref.II], o algoritmo de Deutsch-Josza também serve para demonstrar a importância de permitir que amplitudes quânticas assumam valores positivos e negativos, em oposição às amplitudes clássicas que são sempre não negativas. 
Antes da leitura desse post é recomendado um conhecimento prévio sobre gates quânticos.


## Introdução
Antes de estudar o algoritmo de Deutsch-Josza iremos relembrar dois conceitos importantes para computação quântica: Oracle e o gate Hadamard em n dimensões
#### Oracle
Comecemos pelo conceito de Oracle: dispositivo físico misterioso no sentido de não sabermos as regras que o compoem mas podemos interagir com ele através de queries as quais ele nos retornará respostas. 
Um dos objetivos principais no estudo de Oracles é conseguir respostas relevantes utilizando a quantidade minima de consultas (ou queries) possível.
Qualquer ação em bits, em um computador clássico, pode ser [ref.III] representada através de uma função $$f:\{0,1\}^n \rightarrow \{0,1\}^m$$. Dessa forma nosso Oracle será, por exemplo, a função $$f$$.
Em computação quântica, analogamente, pediremos que nosso Oracle seja reversível e o usaremos diretamente no circuito através do gate simples $$O_f$$ que age através de um gate, isto é, dados dois qubits $$x$$ e $$y$$:

$$
O_f\ket{x}\ket{y} = \ket{x}\ket{y\oplus f(x)}
$$

Para facilitar, por enquanto, trabalharemos apenas com $$f:\{0,1\}^n \rightarrow \{0,1\}$$. Note que se $$f(x)=x$$, então $$O_f$$ será o gate CNOT.
Para facilitar a notação, criaremos outro Oracle $$U_f$$ baseado em $$O_f$$ com $$y=\frac{1}{\sqrt{2}}(\ket{0}-\ket{1})$$ que recebe $$\ket{x}$$ como entrada e retorna $$U_f\ket{x}$$ independentemente de $$y$$.
De fato, note que, com esse $$y$$ teremos,

$$
O_f\ket{x}\ket{y} = \frac{1}{\sqrt{2}}(\ket{x}\otimes  \ket{(\ket{0}-\ket{1})\oplus f(x)}) = \frac{1}{\sqrt{2}}(\ket{x}\otimes \ket{(\ket{0}\oplus f(x)-\ket{1}\oplus f(x))})
$$

que é igual $$\ket{x} \ket{y}$$ no caso em que $$f(x)=0$$ e $$-\ket{x} \ket{y}$$ no caso em que $$f(x)=1$$
Logo,

$$
O_f\ket{x}\ket{y} = (-1)^{f(x)}\ket{x}
$$

E portanto $$U_f$$ é um operador de fase uma vez que $$U_f\ket{x}=(-1)^{f(x)}\ket{x}$$.
O estudo de Oracles não se restringe a este algoritmo. No algoritmo de Grover este conceito também é fundamental.

#### Hadamard gate em n dimensões
Para iniciarmos o estudo do Algoritmo de Deutsch-Josza precisamos, por fim, reelembrar a ação do gate Hadamard em n qubits. O leitor pode verificar facilmente que a ação do gate em um qubit puro $$\ket{0}$$ ou $$\ket{1}$$ é

$$
H\ket{x} = \frac{1}{\sqrt{2}}(\ket{0}+(-1)^x \ket{1})
$$

com $$x = 1$$ ou $$x = 0$$. Daí:

$$
H\ket{x} = \frac{1}{\sqrt{2}}\sum_{k \in \{0,1\}}{}(-1)^{kx}\ket{k}
$$

Para $$x \in \{0,1\}^n$$ teremos:

$$
H^{\otimes n}\ket{x} = \frac{1}{\sqrt{2^n}}\sum_{k \in \{0,1\}^n}{}(-1)^{k*x}\ket{k}
$$

Onde $$*$$ denota o produto interno usual. Por exemplo, se $$n=3$$ e $$\ket{x}=\ket{010}$$ teremos:

$$
H^{\otimes 3}\ket{010} = \ket{+} \otimes \ket{-} \otimes \ket{+} 
$$

$$
= \frac{1}{\sqrt{2}}(\ket{0}+\ket{1}) \otimes \frac{1}{\sqrt{2}}(\ket{0}-\ket{1}) \otimes \frac{1}{\sqrt{2}}(\ket{0}+\ket{1})
$$

$$
= \frac{1}{\sqrt{2^3}}\{(\ket{00}-\ket{01}+\ket{10}-\ket{11}) \otimes (\ket{0}+\ket{1})\}
$$

$$
= \frac{1}{\sqrt{2^3}}\{\ket{000}+\ket{001}-\ket{010}-\ket{011}+\ket{100}+\ket{101}-\ket{110}-\ket{111}\}
$$

$$
= \frac{1}{\sqrt{2^3}}\sum_{k \in \{0,1\}^3}{}(-1)^{k*(010)}\ket{k}
$$

Onde foi aplicado o gate de Hadamard para cada um dos qubits 0,1 e 0, e a soma é para todo $$k$$ da forma $$ijk$$ com $$i,j,k \in \{0,1\}$$



## O Algoritmo de Deutsch-Josza
Seja $$f:\{0,1\}^n \rightarrow \{0,1\}$$ performado por um Oracle tal que $$f(x)$$ seja constante ($$f(x)=0$$ ou $$f(x)=1$$ para todo $$x \in \{0,1\}^n$$) ou balanceado (nesse caso teremos $$f(x)=1$$ e $$f(y)=0$$ por algum $$x$$ e $$y$$ $$\in \{0,1\}^n$$). 
Nosso objetivo é descobrir se $$f$$ é constante ou balanceada. Para isso, o melhor que poderiamos fazer usando computação clássica é chamar, pelo menos, duas vezes o Oracle e caso as respostas sejam diferentes poderiamos afirmar que $$f$$ é balanceada. Entretanto, no pior dos casos, teriamos que fazer $$\frac{N}{2}+1=2^{n-1}+1$$ consultas para detectar a natureza da função, onde $$n$$ é a quantidade de bits de entrada e $$N=2^n$$ é a quantidade total de strings obtidas por estes.
O algoritmo de Deutsch-Josza nos fornece uma forma onde somente 1 consulta é necessária utilizando por causa do fenômeno de superposição.
O circuito quântico, em n=4, que implementa o algoritmo de Deutsch-Josza é:
![Gate-DJ](/assets/images/deutsch-josza-algorithm/dj_circuit.png)
Fonte: Qiskit, refIV. O gate Oracle está representando nosso gate $$U_f$$ construido.


Iremos analisar passo-a-passo o funcionamento do circuito para $$n$$ qualquer.
Inicialmente teremos como entrada o estado puro $$\ket{00..0}$$ onde temos $$n$$ zeros. Nesse caso teremos o estado inicial

$$
\ket{\psi_0}=\ket{0}^{\otimes n}
$$

Após a aplicação dos gates Hadamard teremos, conforme mostrado na introdução:

$$
\ket{\psi_1} = H^{\otimes n}\ket{\psi_0} = \frac{1}{\sqrt{2^n}}\sum_{x \in \{0,1\}^n}{}(-1)^{x*\psi_0}\ket{x} = \frac{1}{\sqrt{2^n}}\sum_{x \in \{0,1\}^n}{}\ket{x}
$$

Onde a última igualdade foi obtida porque $$\ket{\psi_0}=\ket{00...0}$$. Aplicando nosso Oracle teremos:

$$
\ket{\psi_2} = U_f\ket{\psi_1} = \frac{1}{\sqrt{2^n}}\sum_{x \in \{0,1\}^n}{}U_f\ket{x} = \frac{1}{\sqrt{2^n}}\sum_{x \in \{0,1\}^n}{}(-1)^{f(x)}\ket{x}
$$

Por fim, aplicando Hadamard novamente:

$$
\ket{\psi_3} = H^{\otimes n}\ket{\psi_2} = \frac{1}{\sqrt{2^n}}\sum_{x \in \{0,1\}^n}{}(-1)^{f(x)}H^{\otimes n}\ket{x}
$$


$$
= \frac{1}{2^n}\sum_{x \in \{0,1\}^n}{}(-1)^{f(x)}\sum_{k \in \{0,1\}^n}(-1)^{k*x}{}\ket{k}
$$


$$
=\sum_{k \in \{0,1\}^n}{} \{\frac{1}{2^n}\sum_{x \in \{0,1\}^n}{}(-1)^{f(x)+ k*x}\}\ket{k}
$$

Definimos então o termo $$\frac{1}{2^n}\sum_{x \in \{0,1\}^n}{}(-1)^{f(x)+ k*x}$$ como $$c_k$$ e portanto

$$
\ket{\psi_3} = \sum_{k \in \{0,1\}^n}{}c_k \ket{k}
$$

Logo, os termos $$c_k$$ determinarão as probabilidades de medir o estado $$\ket{00...0}$$. De fato:

$$
P[output=00...0] = |\braket{00...0|\psi_3}|^2 = |\sum_{k \in \{0,1\}^n}{}c_k\braket{00...0|k}|^2 = |c_{00...0}|^2
$$

que será igual a 1 se $$k=00...0$$ e 0 caso contrário (pela ortogonalidade). Logo,

$$
P[output=00...0] = |\frac{1}{2^n}\sum_{x \in \{0,1\}^n}{}(-1)^{f(x)}|^2
$$

Neste somatório, podemos ter $$f(x)=0$$ e nesse caso $$\sum_{x \in \{0,1\}^n}{}(-1)^{f(x)} = 2^n$$. Podemos ter também $$f(x)=1$$ e daí: $$\sum_{x \in \{0,1\}^n}{}(-1)^{f(x)} = -2^n$$ ou, por fim, se $$f$$ for balanceada então os termos se cancelam e teriamos zero como resultado. 

Portanto, teremos 1 como probabilidade se $$f$$ for constante e 0 se $$f$$ for balanceada. Dessa forma descobrimos a natureza de $$f$$ rodando apenas uma vez o algoritmo cqd.



### Referências
I. Qiskit - Lecture 2.1 Simple Quantum Algoritms 1 [YouTube] https://www.youtube.com/watch?v=WYAUh-4K5E0&list=PLOFEBzvs-VvqJwybFxkTiDzhf5E11p8BI&index=3 acessado em 06/01/2023

II. Field guide on IBM. Deutsch-Jozsa algorithm, https://quantum-computing.ibm.com/composer/docs/iqx/guide/deutsch-jozsa-algorithm acessado em 06/01/2023

III. [procurar uma fonte pra isso - lembro de ler em algum texto de comp.]

IV. Qiskit https://qiskit.org/textbook/ch-algorithms/deutsch-jozsa.html#4.4-Generalised-Circuits- acessado em 06/01/2023


### Futuro
* 
* 
* 
* 


