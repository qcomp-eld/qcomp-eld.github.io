---
layout: post
title:  "Transformações Quânticas (Matrizes unitárias, Reversibilidade e No-Cloning Theorem)"
date:   2022-08-16 08:00:00
author: fernando_caletti
categories: teoria computacao-quântica educativo
usemathjax: true
excerpt_separator: <!--more-->
---

Quando começamos a avançar no estudo da computação quântica e já temos noção dos conceitos fundamentais isolados como: o que são qubits, estados de qubits, produto entre qubits. Passa a ser necessário também conhecermos melhor as regras e formas de manipulação dos qubits, para que possamos avançar para utilizações mais práticas do conhecimento em computação quântica como a escrita de algoritmos. 

Sendo assim, no presente artigo será abordado diretamente o assunto de Transformações quânticas e suas particularidades para aprofundar o conhecimento nas interações realizadas com qubits em circuitos quânticos.

<!--more-->

### Estados quânticos (Quantum States)
Como já mencionado nos artigos anteriores, a computação quântica é baseada diretamente nas regras da mecânica quântica, o que acaba por trazer uma **abordagem** **probabilísticas** ao invés de determinísticas como na computação clássica. 

Assim sendo, por padrão é utilizada uma representação baseada em **vetores** **de** **estado** para lidar algébricamente com qubits, utilizando operações de álgebra linear.
Para exemplificar, abaixo temos o vetor de estados de um qubit:

$$ \psi = a\ket{0} + b\ket{1} $$

O vetor de estados de um qubit é geralmente representado utilizando a notação bra-ket de Dirac, por podermos retratar de forma genérica um vetor em um espaço com infinitas dimensões, dessa forma tem-se a possibilidade de enxergar de forma simplificada arranjos complexos de estados com probabilidades complexas. O mesmo vetor de estados demonstrado acima também pode ser exibido como:

$$ \psi = \left[ {\begin{array}{cc}
a \\
b \\
\end{array} } \right] $$

Na segunda representação, temos que os coeficientes **a** e **b**, que representam as amplitudes de um determinado estado. O respectivo estado é determinado por sua posição no vetor e os valores de ambos são **complexos**, podendo possuir uma parte real e uma parte imaginária.

Um sistema com dois qubits, como já visto anteriormente, é representado pelo produto tensorial entre os qubits isolados, que acaba por aumentar a quantidade de estados possíveis seguindo a ordem de $$ 2^n $$ estados para **n** qubits, como pode-se visualizar abaixo:

$$ \ket{\psi\phi} = \ket{\psi} \otimes \ket{\phi} $$

Para mais detalhes sobre o cálculo tensorial e entrelaçamento quântico, o assunto é abordado diretamente [nesse post do nosso blog](/matemática/computacao-quântica/produto-tensorial/entrelaçamento/2022/08/06/multiples-qubits-cross-product-entrelacamento.html)!

### Transformação Quântica (Transformações unitárias)
Bom, tendo feito nossa breve retomada da representação de estados quânticos, podemos ir para o assunto principal do artigo, que é justamente a transformação de estados.
Quando nos referimos a transformação quântica, em outras palavras estamos referindo-nos às matrizes de transferência já mencionadas em um [outro artigo](/http://localhost:4000/teoria/computacao-qu%C3%A2ntica/educativo/2022/05/24/portas-unicas.html), que basicamente são matrizes compostas de coeficientes capazes de alterar o estado de um qubit de alguma forma quando aplicadas a ele. A aplicação se dá por meio do calculo do produto de matrizes entre o estado do qubit de entrada e a matriz de transferência.

Por conta da natureza probabilística dos qubits, a natureza não permite informações arbitrárias de suas componentes forçando determinadas regras a serem seguidas ao interagir com seus estados.
A primeira dessas regras é que toda transformação deve ser diretamente conectada às medições quânticas e a superposição quântica.

Sendo o qubit $$ \ket{\psi} = \left[ {\begin{array}{cc}
0 \\
1 \\
\end{array} } \right] $$

E a matriz de transferência H (Hadamard):
$$ 
H =  \frac{1}{\sqrt{2}}\left[ {\begin{array}{cc}
1 & 1 \\
1 & -1\\
\end{array} } \right] 
$$

Temos que $$ \ket{\psi} \rightarrow H = \ket{\psi}H = \ket{-} $$

#### Matrizes Unitárias
Tendo em vista as restrições naturais das operações aplicadas a qubits, temos que elas precisam ser lineares e unitárias.
Mais precisamente, podemos dizer que aplicar uma matriz unitária a um qubit, significa aplicar a mesma matriz a todos os estados possíveis do qubit em superposição.
Da mesma forma, a linearidade implica que o produto escalar seja preservado, assim sendo, o módulo, ou "comprimento" do vetor aplicado não pode ser alterado com a operação, vetores unitários devem manter-se unitários e estados ortogonais devem manter-se ortogonais. (As componentes são complexas).

Assim sendo, existem argumentos matemáticos que implicam que para qualquer transformação quântica **U**, seu operador adjunto precisa ser idêntico ao seu inverso.
$$U^\dagger = U^{-1}$$

Além disso, temos algumas outras definições:
- O produto entre dois operadores unitários é unitário.
- O produto tensorial entre operadores unitários $$ U_1 \otimes U_2 $$ é transformação unitária de $$ X_1 \otimes X_2 $$ somente se $$U_1$$ e $$U_2$$ forem transformações unitárias dos respectivos espaços.

#### Reversibilidade
Devido a natureza incrivelmente frágil dos qubits atuais, um dos grandes problemas enfrentados pela área de computação quântica é o fato de que para reproduzir o comportamento de particulas, o sistema acaba por se tornar sucetível a inúmeros erros e à **decoerência**.

###### Decoerência
Definida como a perda de informação de um qubit (mudança de estado) por conta de interação com o ambiente ou outras partes do sistema.

Portanto, um requisito para transformações e operações quânticas funcionarem em larga escala, é que elas sejam também perfeitamente reversíveis.
Reversibilidade significa sermos capazes de identificar a entrada utilizada somente a partir do conhecimento da saída obtida e da operação realizada, tornando possivel identificar também caso algum tipo de interferencia tenha atingido o sistema no processo.
Como podemos identificar na imagem abaixo, que demonstra a tabela de valores da porta CNOT, que aplica uma inversão de valor no qubit B apenas caso o qubit A esteja em estado 1.

![Reversibilidade 0](/assets/images/transformacoes-quanticas/Reversibility-2.png)
Fonte: [Future Learn](https://www.futurelearn.com/info/courses/intro-to-quantum-computing/0/steps/31561) (2022).

Como pode-se visualizar na imagem acima, a porta CNOT possui duas saídas (A' e B') para tornar-se reversível, pois a saída A' apenas repassa o estado original do qubit A.
A partir disso, pode-se perceber o problema mencionado, pois caso a porta não tivesse a saída A', haveria mais de um estado com a mesma saída e entradas diferentes (1 - 4 e 2 - 3).

### No-Cloning Theorem
Agora, abordaremos um teorema estritamente relacionado às transformações unitárias, o **no-cloning theorem**, que implica que é impossível existir uma operação de clonagem perfeita, que seja capaz de copiar completamente o estado de um qubit em outro.

Supondo que $$U$$ é um operador unitário que é capaz de clonar outros qubits. Assim sendo, temos que:

$$U(\ket{a}\ket{0}) = \ket{a}\ket{a} \Rightarrow$$ para todos os estados quânticos.

Prosseguindo, suponhamos que $$\ket{a}$$ e $$\ket{b}$$ sejam estados ortogonais, e $$U$$ seja capaz de cloná-los livremente, assim sendo:

$$U(\ket{a}\ket{0}) = \ket{a}\ket{a}$$

$$U(\ket{b}\ket{0}) = \ket{b}\ket{b}$$

Agora, considerando que: $$\ket{c} = \frac{1}{\sqrt{2}}(\ket{a} + \ket{b})$$

Por conta da linearidade de transformações quânticas, temos que:

$$U(\ket{c}\ket{0}) = \frac{1}{\sqrt{2}}(U(\ket{a}\ket{0}) + U(\ket{b}\ket{0}))$$

$$U(\ket{c}\ket{0}) = \frac{1}{\sqrt{2}}(\ket{a}\ket{a} + \ket{b}\ket{b})$$

Porém, sendo $$U$$ um operador de clonagem, então obrigatoriamente:

$$U(\ket{c}\ket{0}) = \ket{c}\ket{c}$$

O que implica que:

$$U(\ket{c}\ket{0}) = \ket{c}\ket{c} = \frac{1}{2}(\ket{a}\ket{a} + \ket{a}\ket{b} + \ket{b}\ket{a} + \ket{b}\ket{b})$$

O que é diferente da nossa primeira conclusão sobre o resultado do operador $$U$$.

$$\frac{1}{2}(\ket{a}\ket{a} + \ket{a}\ket{b} + \ket{b}\ket{a} + \ket{b}\ket{b}) \neq \frac{1}{\sqrt{2}}(\ket{a}\ket{a} + \ket{b}\ket{b})$$

Assim sendo, de acordo com as leis da mecânica quântica, é impossível existir um operador unitário capaz de copiar completamente um estado complexo.
Porém, é possível atingir uma eficácia suficientemente utilizável a partir de operações conhecidas, já tendo sido identificado até 80% de precisão em uma cópia durante testes de laboratório.

##### Referências
- FUTURE LEARN. Quantum computing: Reversible evolution. Disponível em: https://www.futurelearn.com/info/courses/intro-to-quantum-computing/0/steps/31561.
- QUANTUM COMPUTING. EdX Quantum Reversibility - Part 1. Disponível em: https://www.youtube.com/watch?v=TpL7gkUGcXU&t=30s.
- Eleanor Rieffel and Wolfgang Polak. 2011. Quantum Computing: A Gentle Introduction (1st. ed.). The MIT Press.
- P.A.M Dirac. 1947. The Principles of Quantum Mechanics (3rd. ed.). University Press, Oxford.