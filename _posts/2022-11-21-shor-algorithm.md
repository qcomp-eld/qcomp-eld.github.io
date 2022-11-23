---
layout: post
title:  "Algoritmo de Shor"
date:   2022-11-21 07:30:00
author: luccasmarim
categories: teoria computacao-quântica educativo
usemathjax: true
---

# Algoritmo de Shor

## O que é?
Algoritmo de decomposição. Dado um número N, encontra os fatores primos que o compõem no produto. Útil para achar decomposições em produto não triviais de números compostos utilizados em criptografia
## Por que estudar?
Utilizando este algoritmo em um computador quântico podemos quebrar, por exemplo, a criptografia RSA em tempo polinomial (mais rápido) [fonte]. Há de se pontuar que fenomenos de quantum noise (apêndice II)[fonte] ou quantum-decoherence (apêndice III)[fonte] interferem na execução deste algoritmo. Atualmente o problema em utilizar este é devido a limitação de hardware[fonte].
## Como funciona?
Dado um número N, nosso problema é equivalente ao problema de encontrar um número divisor não trivial de N entre 1 e N. De fato, achando um número poderiamos executar novamente o algoritmo para encontrar outro número até que N esteja completamente decomposto.
Passos:
1. Descobrir se N é primo ou composto: usar primality-testing (apêndice I)
2. Se N for primo então a única decomposição possível é a trivial: 1 e N. Acabou.
3. Se não, escolha A &isin; &alefsym; qualquer tal que 1<A<N
4. Encontre o MDC (MAIOR divisor comum) de A e N. Seja K este número.
5. Se K&ne;1 então encontramos um fator não trivial. Acabou.
6. Se K=1, então execute a rotina X para encontrar o período r da função $f(x)=A^{x}(mod N)$. Note que isso significa que r é o menor inteiro positivo que satisfaz $A^{r}=1(mod N)$
7. Se r é impar ou $a^{r/2}=-1(mod N)$ então volte para o passo 1.
8. Se não, $MDC(A^{r/2}+1,N)$ ou $MDC(A^{r/2}-1,N)$ são fatores não triviais de N. Acabou.
                                                             
### Rotina X (parte que usa de fato computação quântica para encontrar r):
Antes de começar, introduziremos alguns conceitos importantes para a construção da rotina X.
#### 1. TFQ (Transformada de Fourier Quântica)
A Transformada de Fourier Discreta é um operador linear definido de $C^{N}$ para $C^{N}$ levando $(x_0, x_1, ..., x_{N-1})$ para $(y_0, y_1, ..., y_{N-1})$ tal que:
$$y_k = \frac{1}{\sqrt{N}}\sum_{j=0}^{N-1}x_je^{\frac{2\pi ijk}{N}}$$
Por ser um operador linear unitário (provado em referência II) este tem representação matricial. Na base canônica de $C^{N}$, esta é:
$$\frac{1}{\sqrt{N}}\left(\begin{matrix}
1&1&1&...&1\\
1&\omega&\omega^2&...&\omega^{N-1}\\
1&\omega^2&\omega^4&...&\omega^{N-2}\\
...&...&...&...&...\\
1&\omega^{N-1}&\omega^{N-2}&...&\omega
\end{matrix}\right)$$
Onde $\omega=e^{\frac{2\pi i}{N}}$.
Em mecânica quântica, substituimos os vetores do tipo $(x_0, x_1, ..., x_{N-1})$ da base canônica por kets do tipo $\sum_{j=0}^{N-1}x_j\ket{j}$. Isto é, as coordenadas (ou amplitudes) continuam as mesmas (existe a bijeção) e a base canônica de $C^{N}$ é escrita como kets $\ket{0}$, $\ket{1}$,...,$\ket{N-1}$. Por exemplo, em N=3: $$(1,0,0)^{t} \rightarrow 1\ket{0}+0\ket{1}+0\ket{2} = \ket{0}$$

A construção do circuito que executa a TFQ para qualquer valor de N é um pouco complicada [pesquisar ainda e deixar como apêndice]. A TFQ para $N=2^{n}$ com a base ${\ket{0},..., \ket{2^{n}-1}}$ é:
$$\ket{j}\rightarrow 1/(2^{n/2})\sum_{k=0}^{2^{n}-1}e^{2\pi ijk/(2^{n})}\ket{k}$$
Pode-se mostrar ainda que (referencia III pg39):
$$\ket{j}\rightarrow (\ket{0}+e^{2\pi i0.j_n}\ket{1})(\ket{0}+e^{2\pi i0.j_{n-1}j_n}\ket{1})...(\ket{0}+e^{2\pi i0.j_1j_2...j_n}\ket{1})$$

Podemos [referencia III pg30] construir o circuito que executa essa transformação (exclusiva para $N=2^{n}$): 

![Circuito-TFQ-2n](/assets/images/shor-algorithm/figura3.1_dissertacao.png)
Fonte: [Dissertação de mestrado](https://repositorio.ufmg.br/bitstream/1843/EABA-85FJXP/1/dissertacao_adrianaxavier.pdf) (2010)
Onde H representa o gate Hadamard e $R_k$ controlado definido por:
$$\left(\begin{matrix}
1&0\\
0&e^{\frac{2\pi i}{2^k}}
\end{matrix}\right)$$

#### 2. Grupo $Z_N$ e aritmética modular      
Será apresentado algumas notações importantes para o entendimento do algoritmo:
$$Z_2 = {\overline{0}, \overline{1}} $$
É, por exemplo, o conjunto de todos os números os quais resultam 0 ou 1 na divisão por 2. Isto é, $\overline{0}$ representa os números pares enquanto $\overline{1}$ os impares. Pode-se definir uma operação de multiplicação nesse conjunto de forma que $Z_n = \overline{0}, \overline{1}, ..., \overline{n-1}$ seja um grupo. Definimos também a ordem de um número a como o menor inteiro k tal que $a^{k}=e$ onde $e$ denota o elemento neutro do grupo com respeito a operação de multiplicação
Outra notação importante é a de $A (mod N)$ que é o resto da divisão de A por N (em Python usamos %: A%N)

#### Algoritmo para a Rotina X
Calculemos, primeiramente, a ordem de $x \in Z_N$. Este é o primeiro passo para a rotina X dado um número N que queremos fatorar.
Defina a transformação linear unitária (provado em refIII pag46) $U:C^{n}\rightarrow C^{n}$ por:
$$U\ket{k}=\ket{xk modN}$$
Como construir essa porta????? apendice da dissertacao

##### Circuito da busca de ordem no caso em que a ordem r de um elemento x de $Z_N$ pode ser escrita como $r = 2^{s}$

O primeiro registrador deve possuir $t=[log_2N^2+1]$ qbits no estado $\ket{0}$ e o segundo registrador $L=[log_2N+1]$ qbits no estado $\ket{1}$. [não entendi esses valores t==n?]

![busca-de-ordem-caso-simples](/assets/images/shor-algorithm/figura4.1_dissertacao.png)
Fonte: [Dissertação de mestrado](https://repositorio.ufmg.br/bitstream/1843/EABA-85FJXP/1/dissertacao_adrianaxavier.pdf) (2010)
Começamos por $\ket{\psi_0} = \ket{0...00} \otimes \ket{1}$ o estado inicial conforme o circuito criado.
Aplicamos as portas de Hadamard nos qbits do primeiro registrador resultando em $$\ket{\psi_1} = \frac{1}{\sqrt{2^t}}\sum_{j=0}^{2^{t}-1}\ket{j}\otimes\ket{1}$$
Aplicando as portas $U^{2^j}$ no estado $\ket{\psi_1}$ obtemos: [adicionar o circuito $U^{2^j}$]
$$\ket{\psi_2}= \frac{1}{\sqrt{2^t}}\sum_{j=0}^{2^{t}-1}\ket{j}\otimes\ket{x^j}$$ onde $x$ assume valores de $\ket{0}$ a $\ket{2^t-1}$. [entender melhor essa parte pg 49].
Teremos portanto os termos:
$$\ket{0}\ket{1}, \ket{r}\ket{1}, \ket{2r}\ket{1}, ...,\ket{2^t-r}\ket{1}, $$
uma vez que estamos trabalhando no grupo $Z_N$ e a potência modular é definida por $a^n=a*a*...*a$ (n vezes). O primeiro ket de cada produto tensorial tem esses valores porque trata-se de uma função periódica de período (ou resto) r. [entender melhor]. Note que como ainda não foi realizada a medição então todos os estados de $\ket{\psi_2}$ são equiprováveis.
Rearranjando o estado $\ket{\psi_2}$ obtemos:
$$\ket{\psi_2}=\frac{1}{2^{\frac{t}{2}}}\sum_{b=0}^{r-1}(\sum_{a=0}^{\frac{2^t}{r}-1}\ket{ar+b})\ket{x^b}$$
Realizando uma medição e supondo, sem perda de generalidade, que o resultado tenha sido $\ket{x^{b_1}}$ teremos como estado atual:
$$\ket{\psi_3}=\sqrt{\frac{r}{2^t}}(\sum_{a=0}^{\frac{2^t}{r}-1}\ket{ar+b_1})\ket{x^{b_1}}$$
Para descobrir o valor de $a$ aplicaremos o $TFQ^*$ no primeiro registrador onde * denota o hermitiano da aplicação. Obtemos, conforme (refIII pag50):
$$\ket{\psi_4}=\frac{1}{\sqrt{r}}(\sum_{k=0}^{r-1}e^{-2\pi ikb_1/r}\ket{\frac{k2^t}{r}})\ket{x^{b_1}}$$
E pronto. Podemos realizar a medição na base computacional. Obtendo $\ket{0}$ quando k=0 não teremos informação sobre o sistema mas se o resultado da medição for $\ket{\frac{k2^t}{r}}$ por algum k>0, dividimos por $2^t$ e teremos duas possibilidades: mdc(k,r)=1 ou mdc(k,r) é diferente de 1. No primeiro caso fazemos $x^r = 1 mod N$ e temos a ordem. No segundo caso o denominador será um d que é um fator de r e nesse caso o algoritmo falha.
##### Caso Geral
Os valores de t e L não mudam. O circuito que implementa a busca de ordem no caso geral é:
![busca-de-ordem-caso-geral](/assets/images/shor-algorithm/figura4.2_dissertacao.png)
Fonte: [Dissertação de mestrado](https://repositorio.ufmg.br/bitstream/1843/EABA-85FJXP/1/dissertacao_adrianaxavier.pdf) (2010)
A sequência de estados fica:
$$\ket{\psi_0}=\ket{0}\otimes\ket{1}$$
$$\ket{\psi_1}=\frac{1}{2^{\frac{t}{2}}}\sum_{j=0}^{2^t-1}\ket{j}\otimes\ket{1}$$
$$\ket{\psi_2}=\frac{1}{2^{\frac{t}{2}}}\sum_{j=0}^{2^t-1}\ket{j}\otimes\ket{x^j}$$
$$\ket{\psi_3}=\frac{1}{2^{t}}\sum_{j,k}^{2^t-1}e^{\frac{-2\pi ikj}{2^t}}\ket{k}\otimes\ket{x^j}$$
$$\ket{\psi_4}=\ket{c}\otimes\ket{\alpha_c}$$
com $\ket{c}\in \ket{0}, \ket{1},..., \ket{2^t-1}$ e $\ket{\alpha_c}$ vem da decomposição de $\ket{\psi_3}$ em relação à base computacional do primeiro registrador. 
Para encontrar r com alta probabilidade..........


                                                             
## Apêndice I (primality-testing algorithm):
Algoritmo simples que usa o fato de todos os divisores de um número n serem menores ou iguais a n/2. Em Python:
                                                           
def is_prime(n: int) -> bool:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if n<=3:
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return n>1            
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if n%2 == 0 or n%3 == 0:
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return False
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;limit = int(n**0.5)
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for i in range(5, limit+1, 6):
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if n%i==0 or n%(i+2)==0:
                &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return False                
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return True

    
## Apêndice II (quantum noise)
## Apêndice III (quantum-decoherence)



## Referências
* Wikipedia/eng
* Chapter 5 of QFT, Period Finding & Shor's Algorithm
* Dissertação de Mestrado Adriana Xavier UFMG
* Marcelo Terra Cunha - Curso de meq. quantica para matematicos em formação


# O que falta?
* Implementar no IBM Quantum Composer comparando com o exemplo de dimensão N=4 usado na dissertação pg 41
* Escrever apêndices II e III
* Circuito TFQ para N qualquer
* Escrever algoritmo final
* Testar apêndice I

