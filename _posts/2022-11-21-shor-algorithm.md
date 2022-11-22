---
layout: post
title:  "Algoritmo de Shor"
date:   2022-11-21 07:30:00
author: luccasmarim
categories: teoria computacao-quântica educativo
usemathjax: true
---


# O que é?
Algoritmo de decomposição. Dado um número N, encontra os fatores primos que o compõem no produto. Útil para achar decomposições em produto não triviais de números compostos utilizados em criptografia
# Por que estudar?
Utilizando este algoritmo em um computador quântico podemos quebrar, por exemplo, a criptografia RSA em tempo polinomial (mais rápido) [fonte]. Há de se pontuar que fenomenos de quantum noise (apêndice II)[fonte] ou quantum-decoherence (apêndice III)[fonte] interferem neste algoritmo. Atualmente o problema em utilizar este algoritmo é devido a limitação de hardware[fonte].
# Como funciona?
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
Utilizaremos alguns conceitos para construir a rotina X.
#### TFQ (Transformada de Fourier Quântica)
Notações...
<br>
(referencia 3 pg39):
<br>
Seja N a dimensão (em $C^{n}$) do espaço de representação das amplitudes.
<br>
A construção do circuito para qualquer valor de N é um pouco complicada. A TFQ para $N=2^{n}$ com a base ${\ket{0},..., \ket{2^{n}-1}}$ é:
$$\ket{j}\rightarrow 1/(2^{n/2})\sum_{k=0}^{2^{n}-1}e^{2\pi ijk/(2^{n})}\ket{k}$$
Pode-se mostrar ainda que:
$$\ket{j}\rightarrow (\ket{0}+e^{2\pi i0.j_n}\ket{1})(\ket{0}+e^{2\pi i0.j_{n-1}j_n}\ket{1})...(\ket{0}+e^{2\pi i0.j_1j_2...j_n}\ket{1})$$
<br>
Podemos construir o circuito (exclusivo para $N=2^{n}$) que foi provado na referencia 3 pg39
<br>
![Circuito-TFQ-2n](/assets/images/figura3.1_dissertacao.png)
Fonte: [Dissertação de mestrado](https://repositorio.ufmg.br/bitstream/1843/EABA-85FJXP/1/dissertacao_adrianaxavier.pdf) (2010).
<br>
Onde H representa o gate Hadamard e $R_k$ controlado definido por:
![rk-gate](/assets/images/rkgate.png)
Fonte: [Dissertação de mestrado](https://repositorio.ufmg.br/bitstream/1843/EABA-85FJXP/1/dissertacao_adrianaxavier.pdf) (2010).

$% a matriz de representação da TFD no espaço C^n é unitária$
$% TFQ nada mais é do que a TFD com notação diferente (com bras e kets)$

Notação $Z_N$ e aritmetica modular (ler apêndice IV)                                                        
<br>
Calculemos, primeiramente, a ordem de $x \in Z_N$. Este é o primeiro passo para a rotina X dado um número N que queremos fatorar.
Defina a transformação linear $U:C^{n}\rightarrow C^{n}$ por:
$$U\ket{k}=\ket{xk modN}$$



                                                             
## Apêndice I (primality-testing algorithm):
Algoritmo simples que usa o fato de todos os divisores de um número n serem menores ou iguais a n/2. Em Python:
<br>                                                             
def is_prime(n: int) -> bool:
<br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  if n<=3:
  <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
          return n>1
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  if n%2==0 or n%3==0:
    <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
          return False
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  limit = int(n**0.5)
    <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  for i in range(5, limit+1, 6):
    <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
          if n%i==0 or n%(i+2)==0:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                return False
                  <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  return True
    <br>
    
## Apêndice II (quantum noise)
## Apêndice III (quantum-decoherence)
## Apêndice IV (Aritmética modular)
Será apresentado algumas notações importantes para o entendimento do algoritmo:
$$Z_2 = {\overline{0}, \overline{1}} $$
É, por exemplo, o conjunto de todos os números os quais resultam 0 ou 1 na divisão por 2. Isto é, $\overline{0}$ representa os números pares enquanto $\overline{1}$ os impares. Pode-se definir uma operação de multiplicação nesse conjunto de forma que $Z_n = \overline{0}, \overline{1}, ..., \overline{n-1}$ seja um grupo. Definimos também a ordem de um número a como o menor inteiro k tal que $a^{k}=e$ onde $e$ denota o elemento neutro do grupo com respeito a operação de multiplicação
<br>
Outra notação importante é a de $A (mod N)$ que é o resto da divisão de A por N (em Python usamos %: A%N)


## Referências
* wikipedia/eng
* Chapter 5 of QFT, Period Finding & Shor's Algorithm
* Dissertação de Mestrado Adriana Xavier UFMG


# O que falta?
* Implementar no IBM Quantum Composer comparando com o exemplo de dimensão N=4 usado na dissertação pg 41
* Escrever apêndices II e III
* Circuito TFQ para N qualquer
* Escrever algoritmo final

